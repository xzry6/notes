# Research on images integration

## Tensorflow and Supported Ops
```
|_demo of reading images
|_uploading images
|_what about tf_record
|_supported image format
|_supported operations
```
### Demo of Reading images
```python
import tensorflow as tf

def inputs_image(match_pattern, num_epochs, batch_size, image_h, image_w):
    """This method produce image inputs according to matched image name.
    Args:
        match_pattern: a matching pattern of target images.
        num_epochs: number of epochs.
        batch_size: number of records would be loaded each time.
        image_h: ideal image height.
        image_w: ideal image weight.
    Returns:
        Batch image tensor.
    """
    filenames_queue = tf.train.string_input_producer(
        tf.train.match_filenames_once(match_pattern), num_epochs = num_epochs)
    return _batch_read_image(filenames_queue, batch_size, image_h, image_w)

def _batch_read_image(filenames_queue, batch_size, image_h, image_w):
    """This method reads images through a filenames_queue, reshape the raw image and produce batch images.
    Args:
        filenames_queue: an FIFO queue of file names.
        batch_size: number of records would be loaded each time.
        image_h: ideal image height.
        image_w: ideal image weight.
    Returns:
        Batch image tensor.
    """
    # reader
    image_reader = tf.WholeFileReader()
    image_name, image_file = image_reader.read(filenames_queue)
    # decode and resize
    image = tf.image.decode_jpeg(image_file)
    resized_image = tf.image.resize_images(image, [image_h, image_w])
    resized_image.set_shape((image_h, image_w, 3))
    # shuffle and batch read
    num_preprocess_threads = 1
    min_after_dequeue = 256
    images = tf.train.shuffle_batch([resized_image], batch_size = batch_size,
        num_threads = num_preprocess_threads,
        capacity = min_after_dequeue + 3 * batch_size,
        min_after_dequeue=min_after_dequeue)
    return images

match_pattern = "./images/*.jpg"
batch_size = 50
num_epochs = 50
image_h = image_w = 224

image = inputs_image(match_pattern, num_epochs, batch_size, image_h, image_w)
init_op = tf.group(tf.global_variables_initializer(), tf.local_variables_initializer())

with tf.Session() as sess:
    sess.run(init_op)

    # Start input enqueue threads
    coord = tf.train.Coordinator()
    threads = tf.train.start_queue_runners(coord=coord)

    try:
        while not coord.should_stop():
            image_np = sess.run(image)
            print(image_np.shape)
    except tf.errors.OutOfRangeError:
        print('Done loading')

    coord.request_stop()
    coord.join(threads)
```

### Uploading Images
```
directory structure of labeled image tasks when uploading

|_data-mining-task-1
  |_label-1
    |_pic-1
    |_pic-2
  |_label-2
    |_pic-3
    |_pic-4
  ...
|_data-mining-task-2
  ...
```

### TF Record
tf_record could also be applied via the following way.
Images should be preserved with image itself or with a linked URL in tf_record.
Here is an example of reading a tf_record of images, which preserves whole image infomation .

```python
image_name, image_file = reader.read(filename_queue)
# parse tf_record 
features = tf.parse_single_example(
    image_file,
    features = {
        'height': tf.FixedLenFeature([], tf.int64),
        'width': tf.FixedLenFeature([], tf.int64),
        'image': tf.FixedLenFeature([], tf.string),
        'label': tf.FixedLenFeature([], tf.float32)
    }
)
image = tf.decode_raw(features['image'], tf.uint8)

height = tf.cast(features['height'], tf.int32)
width = tf.cast(features['width'], tf.int32)

image_shape = tf.pack([height, width, 3])
image = tf.reshape(image, image_shape)
```

### Supported Image Format
- GIF
- PNG
- JPEG

### Supported Operations
- Resizing
  - `tf.image.resize_images`
  - `tf.image.resize_area`
  - `tf.image.resize_bicubic`
  - `tf.image.resize_bilinear`
  - `tf.image.resize_nearest_neighbor`

- Cropping
  - `tf.image.resize_image_with_crop_or_pad`
  - `tf.image.central_crop`
  - `tf.image.pad_to_bounding_box`
  - `tf.image.crop_to_bounding_box`
  - `tf.image.extract_glimpse`
  - `tf.image.crop_and_resize`

- Flipping, Rotating and Transposing
  - `tf.image.flip_up_down`
  - `tf.image.random_flip_up_down`
  - `tf.image.flip_left_right`
  - `tf.image.random_flip_left_right`
  - `tf.image.transpose_image`
  - `tf.image.rot90`

- Colorspaces
  - `tf.image.rgb_to_grayscale`
  - `tf.image.grayscale_to_rgb`
  - `tf.image.hsv_to_rgb`
  - `tf.image.rgb_to_hsv`
  - `tf.image.convert_image_dtype`

- Adjustments
  - `tf.image.adjust_brightness`
  - `tf.image.random_brightness`
  - `tf.image.adjust_contrast`
  - `tf.image.random_contrast`
  - `tf.image.adjust_hue`
  - `tf.image.random_hue`
  - `tf.image.adjust_gamma`
  - `tf.image.adjust_saturation`
  - `tf.image.random_saturation`
  - `tf.image.per_image_standardization`

- Bounding Boxes
  - `tf.image.draw_bounding_boxes`
  - `tf.image.non_max_suppression`
  - `tf.image.sample_distorted_bounding_box`

- Denosing
  - `tf.image.total_variation`

## TODO
- research on which storage to choose
- research on how to establish an image server

## Industry images storage architecture
| Company     | Server     | Meta-data Storage     | Images Storage     |
| ----------- | ---------- | --------------------- | ------------------ |
| Instagram   | Gunicorn   | PostgreSQL            | Amazon S3          |
| Facebook    | Some(Java) | MySQL, HBase          | [Haystack](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf) |
| Google      | ／         | ／                    | GFS                |
| Taobao      | /          | /                     | TFS                |

## NGINX
- asynchronous event-driven architecture


## 同步，异步，阻塞，非阻塞
一次IO Read操作包括两个阶段:
1. 等待数据准备
2. 将数据从内核拷贝到进程中

- Synchronous: a synchronous I/O operation causes the requesting process to be blocked until that I/O operation completes
    - Blocking IO: 进程发送`recieve from`后阻塞, 等待数据准备和拷贝之后的返回
    - Non-blocking IO: 进程不停发送`recieve from`, 如果发送时数据正在准备则立即返回, 如果发送时在拷贝数据则等待数据拷贝完成之后返回
    - IO Multiplexing: 配合Non-blocking socket使用, 先发送`select`请求询问数据是否已经准备, 如果返回准备完成则再发送`recieve from`请求进行read
    - Signal-driven IO: 发送请求后进程继续进行任务, 当数据准备完成后返回信号, 用户进程收到信号再发送`recieve from`进行read
- Asynchronous: asynchronous I/O operation does not cause the requesting process to be blocked
    - Asynchronous IO: 进程发送`recieve from`后不阻塞, 数据拷贝成功后返回信号
