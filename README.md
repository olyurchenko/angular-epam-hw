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



