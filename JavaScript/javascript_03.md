# JavaScript_03

## If

```html
<input type="button" value="night" onclick="
  if (document.querySelector('#night_day').value === 'night'){
    document.querySelector('body').style.backgroundColor = 'black';
	document.querySelector('body').style.color = 'white';
	document.querySelector('#night_day').value = 'day';
  } else {
    document.querySelector('body').style.backgroundColor = 'white';
	document.querySelector('body').style.color = 'black';
	document.querySelector('#night_day').value = 'night';
  }
">
```



## While

```javascript
var alist = document.querySelectorAll('a');
var i = 0;
while(i < alist.length){
    alist[i].style.color = 'blue';
    i = i + 1;
}
```



## Function

```javascript
function sum(left, right){
    document.write(left+right+'<br>');
}
```



## Object

### Create

```javascript
var coworkers = {
    "programmer": "egoing",
    "designer": "leezche"
};
document.write("programmer: "+coworkers.programmer+"<br>");
document.write("designer: "+coworkers.designer+"<br>");
coworkers.bookkeeper = "duru";
```

### Read

```javascript
for(var key in coworkers) {
    document.write(key+'<br>');
}
```

### Property & Method

```javascript
coworkers.showAll = function(){
    for(var key in this) {
    	document.write(key+': '+this[key]+'<br>');
	}
}
```


