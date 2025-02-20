## template language 
square brackets indicate javascript:
```html
<app-greeting [message] = "homeMessage()" />
```

## Signals
They need to be called like methods
```typescript
<header>
    <nav>{{title()}}</nav>
</header>

export class HeaderComponent {
  title = signal('my first angular app');
}
```

Without signals (old/traditional) - no need of `()`:
```typescript
<header>
    <nav>{{title}}</nav>
</header>

export class HeaderComponent {
  title = signal('my first angular app');
}
```

Advice: start using Signals

## passing params from parent to child component
parent - import the child
```typescript
@Component({
  selector: 'app-home',
  imports: [GreetingComponent],
  templateUrl: './home.component.html',
  styleUrl: './home.component.scss'
})
export class HomeComponent {
  homeMessage = signal('home message');
}
```
parent - pass param
```typescript
// hardcoded
<app-greeting [message] = "'Hello world2'" />

// using signal
<app-greeting [message] = "homeMessage()" />
```

## event listeners

## Optimizing images (to-do)

Import the NgOptimizedImage directive
```typescript
import {NgOptimizedImage} from '@angular/common';

@Component({
    ...
    imports: [NgOptimizedImage]
});
```

Update the src attribute to be ngSrc

