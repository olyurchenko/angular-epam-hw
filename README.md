Agenda 

 - Angular app decomposition;
 - What is model?
 - Decorators:
    @NgModule;
    @Component,@Directive
    @Input, @Output,
    @ViewChild,@ViewChildren,@ContentChild,
    @ContentChildren
- Attribute Directives: @HostBinding,@HostListener;
- Content Projection;
- Styling Components;
- Components Lifecycle Hooks;
- Change Detection Strategies;
- Dynamic Components Loading;

1 Angular App decomposition

- can has on or more root components; 
- during the component creating we create new tags (how?);

Components decomposition:

Separate your app into functional blocks (button, icon  and so on)

Module decomposition:

Separate your app into logical blocks (auth, posts  and so on)

Module is used to encapsulate the logic  

Could be lazy or not (should be separate )

Shared module can include only components, directives, pipes;


2 Models

-It could be classes and interfaces;

class TaskModel{
    constructor(
        public id: string;
    )
}

(advantages - default values can be initialize)
(disadvantages - prototype includes 3 chains - first - state,second -  behaver, third - object)

interface Task{
    id: string;
}

Naming- in should be a dir, that includes files with such specific templates - something.model.ts;

(advantages - a plain object - first - state and behaver,second - object)

(disadvantages - prototype includes 3 chains)


 3 Decorators:

    @NgModule
        providers? - a set of objects that available in the module. (could be applied to services)

        declarations? - a set of components, directives, and pipes (declarables) that available in this module. (some of them that are used in this module)

        imports? - a set of NgModules whose exported are available to templates in this module.

        exports? - a set of components, directives, and pipes declared in this NgModule that can be used in the template of any component that is part of an NgModule that imports this NgModule. Exported declarations are the module's public API.

        bootstrap? - a set of components that are loaded when this module is loaded. (a root component)

        schemas? - a set of schemas that declare elements to be allowed in the NgModule.

         jit? - when present, this module is ignored by the AOT compiler. It remains in distributed code, and the JIT compiler attempts to compile it at run time, in the browser. To ensure the correct behavior, the app must import !!!


    @Component
        changeDetection? - how changes should be detected;

        viewProviders? - an array of providers that can be used only in the template;

        moduleId? - id of the module that contain the component;

        templateUrl? - a relative path or absolute URL of a template file for an Angular component;

        template? - An inline template for an Angular component;

        styleUrls? - one or more relative paths or absolute URLs for files containing CSS stylesheets to use in this component;

        styles? - one or more inline CSS stylesheets to use in this component;

        animations? - an array of animations (https://angular.io/guide/animations)

        encapsulation? - an encapsulation policy for the component's styling. Possible values:
            ViewEncapsulation.Emulated: apply modified component styles in order to emulate a native Shadow DOM CSS encapsulation behavior. (encapsulated in the template)

            ViewEncapsulation.None: apply component styles globally without any sort of encapsulation.
            (without encapsulation )

            ViewEncapsulation.ShadowDom: use the browser's native Shadow DOM API to encapsulate styles.

        interpolation? - overrides the default interpolation start and end delimiters ({{ and }}).

        schemas? - a set of schemas that declare elements to be allowed in a standalone component;

        imports? - directives, components, and pipes that can be used within its template;

        standalone? - angular components marked as standalone do not need to be declared in an NgModule. Such components directly manage their own template dependencies (components, directives, and pipes used in a template) via the imports property.

        selector? - a selector of the component;

        inputs?: ({
            name: string;
            alias?: string;
            required?: boolean;
            transform?: (value: any) => any;
        } | string)[] - an array of data-bound input properties;

        outputs?: string[] - n array of data-bound outputs;

        exportAs? - defines the name that can be used in the template to assign this directive to a variable;

@Injectable(providedIn ://'root' | 'platform' | 'any' )

        'root' : The application-level injector in most apps;

        'any' : Provides a unique instance in each lazy loaded module while all eagerly loaded modules share one instance. This option is DEPRECATED;

        (All lazy modules will get a a unique instance ,other will get singltone)

        'platform' : A special singleton platform injector shared by all applications on the page; (I don't understand what is this)



What can NOT be used in template (prohibited JS expressions):
    assignments(=,+= ...);
    new, typeof, instanceof;
    ++,--;

Expression context (where we can get date):

    - component instance as a source of binding date;
    - template inputs (let task of ..);
    - template ref (#ref)

What can NOT use in expression
    all global objects (Window, Console ...)

4 Communications between components 

   - input/output communication (binding); (only date owner    should change the date)

        *@Input('identification og the input') class value that will get an input;

        Events:
            - DomEvents (click, kayup ...); (event name  - could be click in brackets are on-click)="functionThatWillBeCalled($event - argument that we will get as a parament )"
            
            Expression context (where we can get date):

                - component instance as a source of binding date;
                - template inputs (let task of ..);
                - template ref (#ref);
                - $event (wrapper of event)

                What can NOT be used in template (prohibited JS expressions):
                - assignments(=,+= ...);
                - new, typeof, instanceof;
                - ++,--;

            Template ref:
                    if it was declared on the dom element we will get a ref of this DOM element;

                    if is was declared on custom element we  will get an access to all public properties of the class;

            - Custom events (used to inform some other components)


   - ViewChild/ViewChildren ContentChild/ContentChildren;

   - exportAs !!!!!

   - services (pull/push strategy);

   - ControlValueAccessor (Forms)



PS. Template ref could be used anywhere in template;

Event sorting => example - <input (keyup.enter)="SomeLogic($event)"/> - will trigger SomeLogic function only if enter will key up


---------------------------------------------------------------

Communication between components;

!if you need to filter some events or some other way to change browser events you can create a directive that will start with the normal event (like click.stop, ans so on) add event emitter to this directive and add host list to this native event (like click) and emit this out put only if the event pass to some condition


!if you need to get access to methods of the directive are some values of the directives properties you can use exportAs in directive decorator add this decorator to some html element in component template and add element ref (#something) with the value that will be equal to exportAs string ;

like 
--------------------------------------------------------------
inside directive
    @Directive({
          selector: '[testSelector]',
          standalone: true,
          exportAs: 'test'
    })

export class ExportAsDirective {
  getValue(): number {
    return 'some value';
  }
}
---------------------------------------------------------------
inside template

<div testSelector #test="test" (click)="onClick(test)">Click and see console!</button>

PS(testSelector attribute is needed to connect HTML element with directive; #test="test" - here we get an exemplar of the directive that can be used through some events (onClick))

---------------------------------------------------------------

Push strategy 

we create 1 component that publish some date 

like

export class SomeComponent  {
  counter = 0;

  constructor(
    private communicatorService: CommunicatorService
  ) {}

  onClick(): void {
    this.counter++;
    this.communicatorService.publishData(this.counter);
  }
}

and we how the date consumer (some another component)

export class SomeOtherComponent implements OnInit, OnDestroy {
  input!: string;

  private sub!: Subscription;

  constructor(private communicatorService: CommunicatorService) {}

  ngOnInit(): void {
    this.sub = this.communicatorService.channel$.subscribe(
      data => this.input = data
    );
  }
  ngOnDestroy(): void {
    this.sub.unsubscribe();
  }
}

The first one publish date to some to some Service;

For this we need to create service with the property type Subject - that can be used as to read date as to write;
PS (without $ )
like data;

And second property that will have an Observable type - to allow a consumer of the date only read them.

PS(used with $)
like date$; Could be converted automatically like this 

export class SomeService {
  // Observable string sources
  private date = new Subject<string>();

  // Observable string streams
  public date$ = this.date.asObservable();

  // Service message commands
  publishData(data: string): void {
    this.date.next(data);
  }
}

//Binding type

 - interpolation (<p>{{Some property of the component}}</p>)

 PS. You should not bind attributes using text interpolation
 (<img src="{{some text}}"/>) - you should bing attribute
  <img [src]="some property" />

  - property binding <img [src]="some property"> -  (one way property from date source to view)

  PS (Attributes !== properties of the HTML element )

  - event binding - (one way property from view to  date source )

  - two way binding <input [(value)]='some property of the component'/>

  classed binding

  dingle class binding

single class binding

  <div [class.test]='shouldTestBeAdded'></div> - if shouldTestBeAdded === true then class test will be added if no, it will be removed 

multiple class binding

  <div [class]='shouldTestBeAdded'></div> - shouldTestBeAdded if a sting of class that should be added

  <div [class]='["test1", "test2"]'></div>  - if the few classes should be added you can pass an array with strings that will contain all classes that you need 


  <div [class]='{test:true}'></div>  - or it could be object

  angular ngClass directive

    <div [ngClass]='first second'></div> 
    <div [ngClass]='[first, second]'></div> 
    <div [ngClass]='{test:true}'></div>

You could use it in the same way as a classes property;

Styles binding

  <div [style.color]='isRed?"red":"black"'></div>
  <div [style.font-size.%]='isBold?"150":"50"'></div>


multiple class binding

  <div [style]='width: 100px (and so on)'></div> 

  <div [style]='["width", "100px", "color", "red"]'></div>   

  <div [class]='{width: isBold?"150":"50"}'></div>  

  angular ngStyle directive
    <div [ngStyle]='{width: isBold?"150":"50"}'></div>

PS Styles with unit like px, % and so on you cold use this units as this one - <div [style.font-size.%]='isBold?"150":"50"'></div> are like this 

<div [style.font-size]='isBold?"150px":"50px"'></div>


Directives (attributive directives could be used to decorate some properties of HTML element ar to change the element behaver )

@Hostbinding and @Hostlitner;

Could be used as to directives as to components

if they used to directive so the behaver of the host will change;

if they used to component so they will change the behaver of all elements with the component selector like app-test

if you need to use some event lisner you should use Hostlitner
it will unsubscribe from listner automatically 

Content projection

Normal

<massage title='cat'> Some text </massage>

//Massage component template

<h1>{{title}}</h1>
<ng-content></ng-content> - here we will get  "Some text"

Multi projections

<massage title='cat'>
    <p data-first> Some text 1 </p>
    <p data-second> Some text 2</p>
</massage>

//Massage component template

<h1>{{title}}</h1>

<ng-content select="[data-first]"></ng-content>("Some text 1")
<ng-content select="[data-second]"></ng-content>("Some text 2")
 - here we will get  our content . Angular will find the appropriate content searching by data attribute

 Content is initialize first and only then view (template)

 Life cycle hooks

 constructor()  - create a class exemplar

 ngOnChanges - called when inputs are changed

 ngOnInit - ones when the component created

 ngDoCheck - called if the input object is changed

 ngAfterContentInit

 ngAfterContentChecked

 ngAfterViewInit

 ngAfterViewChecked

 ngOnDestroy