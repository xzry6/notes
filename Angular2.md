# Note: Angular2
This is a learning note of angular2 framework
### Agenda
* [Structure](#structure)
* [Settings](#settings)
  * [package.json](#packagejson)
  * [tsconfig.json](#tsconfigjson)
  * [typings.json](#typingsjson)
* [MVVM](#mvvm)
  * [index.html](#indexhtml)
  * [app/main.ts](#maints)
  * [app/*.ts](#ts)
  * [app/*.component.ts](#componentts)
  * [app/*.service.ts](#servicets)
* [*NEW* Router](#router)
  * [RouteConfig](#routeconfig)
  * [ROUTER_PROVIDERS](#router_providers)
  * [ROUTER_DIRECTIVES](#router_directives)
  * [Location](#location)

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
### MVVM
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
#### *.ts
Create an interface or a model.
```
export interface Hero {
  id: number;
  name: string;
}
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
#### *.service.ts
Service connects models and components.<br>
Import 'Injectable' decorator in your service.
```
import {Injectable} from 'angular2/core';
@Injectable()
export class HeroService { }
```
Import and provide your service in the component.
```
import {HeroService} from './hero.service';
```
```
@Component({
 providers: [HeroService]
)}
```
User a constructor to declare the service.
```
constructor(private _heroService: HeroService) { }
```
Inject the service.
```
this.heroes = this._heroService.getHeroes();
```
We can use Promise in service methods.
```
getHeroes() {
  return Promise.resolve(HEROES);
}
```
And inject service in this way.
```
this._heroService.getHeroes().then(heroes => this.heroes = heroes);
```

### Router
In current version, Angular2 uses router to navigate from one view to the next.<br>
Here is the structure of route in Angular2.
![RouterStructure](http://52.8.152.237/sean/wp-content/uploads/2016/03/router.png)
##### RouteConfig
Use RouteConfig to define route.
```
import {RouteConfig, Route} from 'angular2/router';
 
@RouteConfig([
  new Route({path: '/home', component: HomeCmp, name: 'HomeCmp' })
])

class MyApp {}
```
RouteConfig construct a RouteDefinition class.
```
@CONST()
export class RouteConfig {
  constructor(public configs: RouteDefinition[]) {}
}
```
RouteDefinition interface in route_definition.ts
```
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
##### ROUTER_PROVIDERS
ROUTER_PROVIDERS gives a list of Providers. ROUTER_PROVIDERS must be included in the app or bootstrapped.<br>
```
import {ROUTER_PROVIDERS} from 'angular2/router';
import {AppCmp} from './app.component';

bootstrap(AppCmp, [ROUTER_PROVIDERS]);
```
ROUTER_PROVIDERS import a ROUTER_PROVIDERS_COMMON class as below.
```
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
##### ROUTER_DIRECTIVES
RouterLink and RouterOutlet are included in ROUTER_DIRECTIVES. RouterLink navigates user to the view while RouterOutlet shows the view in the page.<br>
Use RouterLink in the following way.
```
import {ROUTER_DIRECTIVES} from 'angular2/router';
```
```
<a [routerLink]="['./User']">link to user component</a>
```
RouteLink calls Instruction to navagate.
```
private _navigationInstruction: Instruction;

  constructor(private _router: Router, private _location: Location) {
    // we need to update the link whenever a route changes to account for aux routes
    this._router.subscribe((_) => this._updateLink());
  }

  // because auxiliary links take existing primary and auxiliary routes into account,
  // we need to update the link whenever params or other routes change.
  private _updateLink(): void {
    this._navigationInstruction = this._router.generate(this._routeParams);
    var navigationHref = this._navigationInstruction.toLinkUrl();
    this.visibleHref = this._location.prepareExternalUrl(navigationHref);
  }
```
RouteOutlet is used as a tag in views.
```
<router-outlet></router-outlet>
```

##### Location
Location is a service that applications can use to interact with a browser's URL.
```
import {Location} from 'angular2/router';
```
```
class AppCmp {
  constructor(location: Location) {
    location.go('/foo');
  }
}
```
Location derives from LocationStrategy.
```
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

Provide a provider to a string URL prefix in PathLocationStrategy.
```
bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  provide(APP_BASE_HREF, {useValue: '/my/app'})
]);
```
When using HashLocationStrategy.
```
bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  provide(LocationStrategy, {useClass: HashLocationStrategy})
]);
```
LocationStrategy uses PlatformLocation to encapsulate calls to DOM apis. PlatformLocation has following structure.
```
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
