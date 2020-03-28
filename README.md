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
    factory.componentType.prototype['action']("arg1", "arg2");
  ```
  
  
