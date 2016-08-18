# Router
Here is some note of Angular2 router.

### Agenda
* [Structure](#structure)
* [RouteConfig](#routeconfig)
* [ROUTER_PROVIDERS](#router_providers)
* [ROUTER_DIRECTIVES](#router_directives)
* [Location](#location)

### Structure
In current version, Angular2 uses router to navigate from one view to the next.<br>
Here is the structure of route in Angular2.
![RouterStructure](http://52.8.152.237/sean/wp-content/uploads/2016/03/router-1.png)

### RouteConfig
Use RouteConfig to config route settings.
```typescript
import {RouteConfig, Route} from 'angular2/router';
 
@RouteConfig([
  new Route({path: '/home', component: HomeCmp, name: 'HomeCmp' })
])

class MyApp {}
```
RouteConfig implements RouteDefinitin and then post these definitions to RouteRegistry.
![RouteConfig](http://52.8.152.237/sean/wp-content/uploads/2016/03/routerConfig.png)
RouteConfig is an array of RouteDefinition(s).
```typescript
@CONST()
export class RouteConfig {
  constructor(public configs: RouteDefinition[]) {}
}
```
RouteDefinition interface in route_definition.ts
```typescript
export interface RouteDefinition {
  path?: string;
  aux?: string;
  regex?: string;
  serializer?: RegexSerializer;
  component?: Type | ComponentDefinition;
  loader?: () => Promise<Type>;
  redirectTo?: any[];
  as?: string;
  name?: string;
  data?: any;
  useAsDefault?: boolean;
}
```
RouteConfig configs RouteRegistry.
```typescript
configFromComponent(component: any): void {
  if (!isType(component)) {
    return;
  }
  if (this._rules.has(component)) {
    return;
  }
  var annotations = reflector.annotations(component);
  if (isPresent(annotations)) {
    for (var i = 0; i < annotations.length; i++) {
      var annotation = annotations[i];

      if (annotation instanceof RouteConfig) {
        let routeCfgs: RouteDefinition[] = annotation.configs;
        routeCfgs.forEach(config => this.config(component, config));
      }
    }
  }
}
```

### ROUTER_PROVIDERS
ROUTER_PROVIDERS gives a list of Providers. ROUTER_PROVIDERS must be included in the app or bootstrapped.<br>
```typescript
import {ROUTER_PROVIDERS} from 'angular2/router';
import {AppCmp} from './app.component';

bootstrap(AppCmp, [ROUTER_PROVIDERS]);
```
ROUTER_PROVIDERS provides following settings.
![RouteProviders](http://52.8.152.237/sean/wp-content/uploads/2016/03/routerProviders.png)
```typescript
export const ROUTER_PROVIDERS_COMMON: any[] = CONST_EXPR([
  RouteRegistry,
  CONST_EXPR(new Provider(LocationStrategy, {useClass: PathLocationStrategy})),
  Location,
  CONST_EXPR(new Provider(
      Router,
      {
        useFactory: routerFactory,
        deps: CONST_EXPR([RouteRegistry, Location, ROUTER_PRIMARY_COMPONENT, ApplicationRef])
      })),
  CONST_EXPR(new Provider(
      ROUTER_PRIMARY_COMPONENT,
      {useFactory: routerPrimaryComponentFactory, deps: CONST_EXPR([ApplicationRef])}))
]);
```

### ROUTER_DIRECTIVES
RouterLink and RouterOutlet are included in ROUTER_DIRECTIVES. RouterLink navigates user to the view while RouterOutlet shows the view in the page.<br>
Use RouterLink in the following way.
```typescript
import {ROUTER_DIRECTIVES} from 'angular2/router';
```
```html
<a [routerLink]="['./User']">link to user component</a>
```
RouteOutlet is used as a tag in views.
```html
<router-outlet></router-outlet>
```
RouteLink and RouteOutlet calls Router to generate an instruction.
![RouteDirectives](http://52.8.152.237/sean/wp-content/uploads/2016/03/routerDirectives.png)

### Location
Location is a service that applications can use to interact with a browser's URL.
```typescript
import {Location} from 'angular2/router';
```
```typescript
class AppCmp {
  constructor(location: Location) {
    location.go('/foo');
  }
}
```
Location derives from LocationStrategy.
```typescript
constructor(public platformStrategy: LocationStrategy) {
  var browserBaseHref = this.platformStrategy.getBaseHref();
  this._baseHref = stripTrailingSlash(stripIndexHtml(browserBaseHref));
  this.platformStrategy.onPopState((ev) => {
    ObservableWrapper.callEmit(this._subject, {'url': this.path(), 'pop': true, 'type': ev.type});
  });
}
```
LocationStrategy represents route state. Angular2 provides two strategies: HashLocationStrategy PathLocationStrategy(default).<br>
* HashLocationStrategy: http://example.com#/foo
* PathLocationStrategy: http://example.com/foo
<br>
<br>
Several options are included in Location.
```typescript
/**
   * Returns the normalized URL path.
   */
  path(): string { return this.normalize(this.platformStrategy.path()); }
/**
   * Changes the browsers URL to the normalized version of the given URL, and pushes a
   * new item onto the platform's history.
   */
  go(path: string, query: string = ''): void {
    this.platformStrategy.pushState(null, '', path, query);
  }

  /**
   * Changes the browsers URL to the normalized version of the given URL, and replaces
   * the top item on the platform's history stack.
   */
  replaceState(path: string, query: string = ''): void {
    this.platformStrategy.replaceState(null, '', path, query);
  }

  /**
   * Navigates forward in the platform's history.
   */
  forward(): void { this.platformStrategy.forward(); }

  /**
   * Navigates back in the platform's history.
   */
  back(): void { this.platformStrategy.back(); }

  /**
   * Subscribe to the platform's `popState` events.
   */
````
Provide a provider to a string URL prefix in PathLocationStrategy.
```typescript
bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  provide(APP_BASE_HREF, {useValue: '/my/app'})
]);
```
When using HashLocationStrategy.
```typescript
bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  provide(LocationStrategy, {useClass: HashLocationStrategy})
]);
```
LocationStrategy uses PlatformLocation to encapsulate calls to DOM apis. PlatformLocation has following structure.
```typescript
export abstract class PlatformLocation {
  abstract getBaseHrefFromDOM(): string;
  abstract onPopState(fn: UrlChangeListener): void;
  abstract onHashChange(fn: UrlChangeListener): void;

  /* abstract */ get pathname(): string { return null; }
  /* abstract */ get search(): string { return null; }
  /* abstract */ get hash(): string { return null; }

  abstract replaceState(state: any, title: string, url: string): void;

  abstract pushState(state: any, title: string, url: string): void;

  abstract forward(): void;

  abstract back(): void;
}
```
