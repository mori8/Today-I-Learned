### div를 img로 예쁘게 꽉 채우는 방법

#### html
```html
<div className="project--image--wrapper">
    <img src="background.jpg"/>
</div>
```

#### css
```css
.project--image--wrapper {
  height: 22.5rem;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.project--image--wrapper img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}
```
