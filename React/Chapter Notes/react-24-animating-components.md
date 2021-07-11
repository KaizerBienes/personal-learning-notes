# CSS Animations
- CSS Animations and why they are sometimes not enough
- animating react components with extra libraries


### Working with CSS Animations
1. using native CSS
- animating like this is fine
- native css features tend to be quite performing
```css
.Modal {
    transition: all 0.3s ease-out;
}

.ModalOpen {
    display: block;
    opacity: 1;
    transform: translateY(0);
}

.ModalClosed {
    display: none;
    opacity: 0;
    transform: translateY(-100%);
}
```
2. using CSS keyframes
- `forwards` prevents looping
```css
.ModalClosed {
    animation: closeModal 0.4s ease-out forwards;
}

@keyframes closeModal {
    0% {
        opacity: 1;
        transform: translateY(0);
    }
    50% {
        opacity: 1;
        transform: translateY(20%);
    }
    100% {
        opacity: 0;
        transform: translateY(-100%);

    }
}
```
- removing happens instantly which is why animation for the exit does not play

### Tools for CSS Animations
- `npm install react-transition-group`
- https://reactcommunity.org/react-transition-group/
- smoothly animate and then remove from don

### Alternative Animation Classes
- React-Motion
    - define start and end via real-world physics
- React-Move
    - two basic components
- React-router-transition
    - animation between different routes