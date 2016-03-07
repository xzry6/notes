# note-angular2
Learning note of angular2 framework
### Agenda
* [Structure](#structure)<br>
* [Settings](#settings)<br>
  * [package.json](#packagejson)<br>
  * [tsconfig.json](#tsconfigjson)<br>
  * [typings.json](#typingsjson)<br>
* [Details](#details)<br>
  * [index.html](#indexhtml)<br>
  * [app/main.ts](#maints)<br>
  * [app/*.component.ts](#componentts)<br>

### Structure
* a regular angular2 app
  * app (main operation of app)
    * app.component.ts
    * main.ts (main file)
  * node_modules ... (modules of node)
  * typings ... (type script definitions)
  * index.html (homepage)
  * package.json (npm settings)
  * tsconfig.json (type script settings)
  * typings.json (type script definitions settings)
  <br>

### Settings
#### package.json
Settings of npm, including name, version, script, dependencies, etc.
```
{
  "name": "angular2-quickstart",
  "version": "1.0.0",
  "scripts": {
    "start": "concurrent \"npm run tsc:w\" \"npm run lite\" ",    
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "lite": "lite-server",
    "typings": "typings",
    "postinstall": "typings install" 
  },
  "license": "ISC",
  "dependencies": {
    "angular2": "2.0.0-beta.7",
    "systemjs": "0.19.22",
    "es6-promise": "^3.0.2",
    "es6-shim": "^0.33.3",
    "reflect-metadata": "0.1.2",
    "rxjs": "5.0.0-beta.2",
    "zone.js": "0.5.15"
  },
  "devDependencies": {
    "concurrently": "^2.0.0",
    "lite-server": "^2.1.0",
    "typescript": "^1.8.2",
    "typings":"^0.6.8"
  }
}
```
#### tsconfig.json
Settings of typescript, including compilierOptions, exclude, etc.
```
{
  "compilerOptions": {
    "target": "es5",
    "module": "system",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "exclude": [
    "node_modules",
    "typings/main",
    "typings/main.d.ts"
  ]
}
```
#### typings.json
Settings of type script definitions.
```
{
  "ambientDependencies": {
    "es6-shim": "github:DefinitelyTyped/DefinitelyTyped/es6-shim/es6-shim.d.ts#6697d6f7dadbf5773cb40ecda35a76027e0783b2"
  }
}
```
### Details
#### index.html
When an angular2 app is started, index.html in main menu is loaded. In the index.html, please include introduced .css and .js file.
```
<link rel="stylesheet" href="styles.css">
<script src="node_modules/es6-shim/es6-shim.min.js"></script>
<script src="node_modules/systemjs/dist/system-polyfills.js"></script>
<script src="node_modules/angular2/es6/dev/src/testing/shims_for_IE.js"></script>

<script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>
<script src="node_modules/systemjs/dist/system.src.js"></script>
<script src="node_modules/rxjs/bundles/Rx.js"></script>
<script src="node_modules/angular2/bundles/angular2.dev.js"></script>
```
Then configure SystemJS with main.ts file.
```
<script>
 System.config({
  packages: {
   app: {
    format: 'register',
    defaultExtension: 'js'
   }
  }
 });
 System.import('app/main')
   .then(null, console.error.bind(console));
 </script>
```
Use a tag which is defined in one of your component's selector to display the page.
```
<body>
 <my-app>Loading</my-app>
</body>
```
#### main.ts
main.ts is the main file called in index.html. Main component should be bootsrapped in main.ts.
```
import {bootstrap}            from 'angular2/platform/browser'
import {AppComponent}         from './app.component'
import {HeroDetailComponent}  from './hero-detail.component'

bootstrap(AppComponent);
bootstrap(HeroDetailComponent);
```
#### *.component.ts
Related components should be imported first.
```
import {Component}            from 'angular2/core';
import {Hero}                 from './hero';
import {HeroDetailComponent}  from './hero-detail.component';
```
*.component.ts could be displayed as a component of a page by replacing a tag in index.html or other component with its content. 'selector' value in '@Component' defines the replaced tag. 'template' is the content page of replaced value. main component should be bootstrapped in main.ts.
```
@Component({
 selector: 'my-hero-detail',
 template: `    
  <ul class="heroes">
   <li *ngFor="#hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
     <span class="badge">{{hero.id}}</span> {{hero.name}}
   </li>
  </ul>
 `
})
```
The class in the *.component.ts is the model and controller of this view.
```
export class HeroDetailComponent {
  heroes = HEROES;
  selectedHero: Hero;
  onSelect(hero: Hero) { this.selectedHero = hero; }
}
```
Imported input should be listed.
```
inputs: ['hero']
```
Imported component should be listed.
```
directives: [HeroDetailComponent]
```
