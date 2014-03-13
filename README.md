Angular-listview
================

simple but flexible listview module for angular.js, it makes manage a listview with angular.js fairly easy

check this http://codepen.io/yaoyi/pen/xbKAn to get the complete demo

## For example

assume that you have a list of data:

```javascript
$scope.items = [
    {
      title: "title1",
      date: new Date(),
      filesize: "45678",
      width: "400",
      height: "600",
      tags: ['wedding','travel'],
      thumb: 'cover1.jpg'
    },
    {
      title: "title2",
      date: new Date(),
      filesize: "1567802",
      width: "300",
      height: "500",
      tags: ['wedding'],
      thumb: 'cover2.jpg'
    },
    {
      title: "title3",
      date: new Date(),
      filesize: "4567822",
      width: "400",
      height: "500",
      tags: ['travel', 'family'],
      thumb: 'cover3.jpg'
    }
  ]
```

you start with showing title/date/filesize, for some reason, you want to add width column to the listview,
in this case you have to modify the listview template, even worse if multi listview instances required in one project, which may have different column needs, that would be troublesome. so let's keep it dry with this module

### Basic Setup

1.include files

```html
<link rel="stylesheet" href="css/listview.css" media="screen" type="text/css">
<script src="http://cdnjs.cloudflare.com/ajax/libs/angular.js/1.2.10/angular.min.js"></script>
<script src="js/listview.js"></script>
```

2.add codes below

```html
<div 
  data-listview=""
  data-items="items"
  data-columns="title, date, filesize">
</div>
```

```javascript
var app = angular.module('demo', ['ui.listview']);
app.controller('DemoCtrl', ['$scope', function($scope){
}])
```

then if needs width info, just change 

```html
<div 
  data-listview=""
  data-items="items"
  data-columns="title, date, filesize">
</div>
```
to 

```html
<div 
  data-listview=""
  data-items="items"
  data-columns="title, date, filesize, width">
</div>
```

### Customize Column

the module allows you to customize the column by adding column templates and passing methods.

#### custom methods for columns

for example, use custom method to format the date column.

1.add methods in your controller

```javascript 
function formatDate(dateStr) {
    var datetime = new Date(dateStr);
    var year = datetime.getFullYear();
    var month = datetime.getMonth();
    var date = datetime.getDate();
    var hours = datetime.getHours();
    var minutes = datetime.getMinutes();

    var ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12;
    minutes = minutes < 10 ? '0'+minutes : minutes;

    return date + '/' + month + '/' + year + ' ' + hours + ':' + minutes + ampm;
  }
```

2.add this method to $scope.listview

```javascript
  $scope.listview = {}
  $scope.listview.methods = {
    date: formatDate
  }
```

3.pass listview.methods to module

```html
<div 
  data-listview=""
  data-items="items"
  data-columns="title, date, filesize"
  data-methods="listview.methods">
</div>
```

#### custom column style by adding templates to listview.html

change color of date column to red

```html
<script type="text/ng-template" id="column-date.html">
  <span style="color:red;">{{_format(column,item)}}</span>
</script>
```

or change styles of tag column

```html
<script type="text/ng-template" id="column-tags.html">
  <div ng-repeat="tag in item[column]" class="tag">
    <span>{{tag}}</span>
  </div>
</script>
```


## License

The MIT License

Copyright (c) 2013 yaoyi, https://github.com/yaoyi

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
  


