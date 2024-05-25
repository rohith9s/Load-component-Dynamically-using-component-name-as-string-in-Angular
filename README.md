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
    factory.componentType.prototype['action']("arg1", "arg2");    <------------ invoking action(arg1, arg2)
  ```
  
  ## Problem Statement inspired to dig above solution-
  Lets say we have a requirement to develop custom angular libraries and resuse them in our runnable applications and where we have a requirement to invoke a method resided in runnable application from one of the custom library, but which leads circular dependency and a useless mess. so instead of circular dependency we will [Pass Data to Library using forRoot](https://medium.com/@michelestieven/angular-writing-configurable-modules-69e6ea23ea42) as below 
  
  ```
    libModule.forRoot(componentsConfigMap)
  ```
 # Load Service Dynamically using service name as string in Angular
 
  where runnable application `componentsConfigMap` config is passed to library module and used to create instance and invoke methods with parameters dynamically.



To dynamically load and invoke methods from services in Angular 15, you can use a similar approach to what you've used for components. Here’s how you can do it:

1. **Create a configuration map for your services**:

   ```typescript
   import { Injectable, Injector } from '@angular/core';
   import { Service1 } from './services/service1.service';
   import { Service2 } from './services/service2.service';
   import { Service3 } from './services/service3.service';

   export const servicesConfigMap = {
     'serviceIdentifier1': Service1,
     'serviceIdentifier2': Service2,
     'serviceIdentifier3': Service3
   };
   ```

2. **Create a method to resolve and invoke the service dynamically**:

   ```typescript
   @Injectable({
     providedIn: 'root'
   })
   export class DynamicServiceInvoker {
     constructor(private injector: Injector) {}

     invokeService(serviceIdentifier: string, methodName: string, ...args: any[]) {
       const service = this.injector.get(servicesConfigMap[serviceIdentifier]);
       if (service && service[methodName]) {
         return service[methodName](...args);
       } else {
         throw new Error(`Method ${methodName} not found on service ${serviceIdentifier}`);
       }
     }
   }
   ```

3. **Example usage**:

   In your component or another service where you want to invoke the dynamic service:

   ```typescript
   import { Component } from '@angular/core';
   import { DynamicServiceInvoker } from './dynamic-service-invoker.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent {
     constructor(private dynamicServiceInvoker: DynamicServiceInvoker) {}

     invokeDynamicService() {
       this.dynamicServiceInvoker.invokeService('serviceIdentifier1', 'methodName', 'arg1', 'arg2')
         .then(result => console.log('Service method result:', result))
         .catch(error => console.error('Error invoking service method:', error));
     }
   }
   ```

In this example, you dynamically resolve a service based on its identifier and invoke a method on it with the provided arguments. This approach uses Angular's `Injector` to get an instance of the service and then calls the specified method dynamically. 

Ensure that your services are properly defined and have the methods you want to invoke. For instance:

```typescript
@Injectable({
  providedIn: 'root'
})
export class Service1 {
  methodName(arg1: any, arg2: any) {
    console.log('Service1 method invoked with', arg1, arg2);
    return Promise.resolve('Result from Service1');
  }
}
```

This setup allows you to dynamically load and call methods on services in a manner similar to how you dynamically load components.
