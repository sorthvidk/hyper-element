# hyperElement

A combining the best of [hyperHTML] and [Custom Elements]!

You custom-elements is build with hyperHTML and will be re-rendered on attribute and store change.

To define a Custom Element

```js
document.registerElement("my-elem", class extends hyperElement{

  render(Html){
    Html`hello world`
  }

})
```

To use your Custom Element

```html
<my-elem/>
```

# Api

## funtions

There are 3 functions. `render` is *required* and `watch` & `readStore` are *optional*

### render

This is what will be displayed with in your element

```js
render(Html,store){

    Html`
      <h1>
          Lasted updated at ${new Date().toLocaleTimeString()}
      </h1>
    `
}
```

### watch

The `watch` function should returns an onNext function to fire a render

Example of re-rendering every second

```js
watch(){
    return (next)=>{
      setInterval(next, 1000);
    }
}
```

### readStore

The `readStore` function should returns the current data-set

Example of re-rendering every second

```js
readStore(){
    return app.store
}
```


## this

* this.props : the attribute on the tage `<my-elem min="0" max="10" />`
* this.store : the value returned from the store function. !only update before each render
* this.textContent : the content between the tags `<my-elem>Hi!</my-elem>`


# Example

## Backbone

```js
var user = new (Backbone.Model.extend({
    defaults: {
        name: 'Guest User',
    }
}));

document.registerElement("my-profile", class extends hyperElement{

  watch(){ return user.on.bind(user,"change")  }
  store(){ return user.toJSON() }

  render(Html,{name}){
    Html`Profile: ${name}`
  }
})
```

## mobx

```js
const user = observable({
  name: 'Guest User'
})

document.registerElement("my-profile", class extends hyperElement{

  watch(){ return mobx.autorun  }
  store(){ return user }

  render(Html,{name}){
    Html`Profile: ${name}`
  }
})
```

[hyperHTML]:https://viperhtml.js.org/hyper.html
[Custom Elements]:https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements