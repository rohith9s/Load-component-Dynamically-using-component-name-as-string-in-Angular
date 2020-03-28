# Load component Dynamically using component name as string in Angular

Tradionally in javascript we call functions dynamically using window object as below: 
 ```
  window[stringFunction](param); 
 ```
 
 But where as in angular to achieve this below is the way:
 
 ```typescript
  export const componentsConfigMap = {
    'compIdentifier1': ComponentName1,
    'compIdentifier2': ComponentName2,
    'compIdentifier3': ComponentName3
  };
  ```
  
  `ComponentName1` component holds below code:
  
  ```typescript
    export class ComponentName1 {
       constructor() { } 
       action(arg1, arg2) {
          console.log("****ComponentName1 - action(- , -) invoked with args****:", arg1, arg2);
       }
    }
  ```
  
  To instantiate `ComponentName1` and invoke a `action()` with params using ComponentfactoryResolver as below:
  
  ```typescript
    const factory = this.resolver.resolveComponentFactory(componentsConfigMap.compIdentifier1);
    factory.componentType.prototype['action']("arg1", "arg2");                             <------------ invoking action(arg1, arg2)
  ```
  
  ## Problem Statement inspired to dig above solution-
  Lets say we have a requirement to develop custom angular libraries and resuse them in our runnable applications and where we have a requirement to invoke a method resided in runnable application from one of the custom library, but which leads circular dependency and a useless mess. so instead of circular dependency we will [Pass Data to Library using forRoot](https://medium.com/@michelestieven/angular-writing-configurable-modules-69e6ea23ea42) as below 
  
  ```
    libModule.forRoot(componentsConfigMap)
  ```
  
  where runnable application `componentsConfigMap` config is passed to library module and used to create instance and invoke methods with parameters dynamically.
