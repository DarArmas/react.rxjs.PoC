# Project: Proof of Concept - Component Communication with RxJS

## Description

This project is a proof of concept demonstrating how information can be shared between two React components using the `rxjs` package. The goal is to show how one component can send information and how another component can react to that information by updating its state accordingly.

### Project Structure

The project consists of two main components:

- **Component1**: This component sends information using a service that implements RxJS.
- **Component2**: This component receives the information sent by `Component1` and updates its state accordingly.

### Prerequisites

To run this project, you need to have Node.js and npm installed. Additionally, you should ensure that the `rxjs` package is installed in your project. If not, you can install it using the following command:

```bash
npm install rxjs
Installation
Clone this repository.
Navigate to the project folder.
Run npm install to install the necessary dependencies.
```

# Functionality
The project uses a shared service, sharingInformationService, which acts as an intermediary for sharing data between components. This service is implemented using RxJS Subject, allowing any component that subscribes to it to receive updates.

### Component 1
Component1 contains two buttons that send different values to the service:

```javascript
Copiar código
import { sharingInformationService } from "../services";

function Component1() {
  const handleClick = () => {
    sharingInformationService.setSubject(true);
  };

  const handleClickNo = () => {
    sharingInformationService.setSubject(false);
  };
  return (
    <div>
      <button onClick={handleClick}>Send Information</button>
      <button onClick={handleClickNo}>Do NOT Send Information</button>
    </div>
  );
}

export default Component1;
```
The first button (Send Information) sends the value true.
The second button (Do NOT Send Information) sends the value false.


### Component2
Component2 subscribes to the sharingInformationService and reacts to the received data:

``` js
Copiar código
import { useEffect, useState } from 'react';
import { sharingInformationService } from '../services';

function Component2() {
  const [count, setCount] = useState(0);
  const subscription$ = sharingInformationService.getSubject();

  useEffect(() => {
    const subscription = subscription$.subscribe(data => {
      if (!!data) setCount(count + 1);
    });

    return () => subscription.unsubscribe(); // Clean up subscription
  }, [count]); 

  return <div>{count}</div>;
}

export default Component2;
```
Component2 subscribes to the Subject provided by sharingInformationService. Each time it receives a true value, it increments the count.
The subscription is cleaned up when the component unmounts, preventing memory leaks.