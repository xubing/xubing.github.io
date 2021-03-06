---
layout:  post
title:  React Native common cell item
tags: reactnative 
description: 一个通用组件,react-native-common-item-cell
comments: true
---

## 这是一个fork别人的工程，然后稍微修改的组件。主要学习下组件的npm发布，以及react native的组件。
[github地址](https://github.com/xubing/react-native-common-item-cell/blob/master/README.md)

# react-native-common-item-cell
A React Native  item cell. The cell grows with the inner text.
This project is forked from [react-native-item-cell](https://github.com/APSL/react-native-item-cell), I add the left title in it .

<p align="center">
<img src="https://raw.githubusercontent.com/xubing/react-native-common-item-cell/master/Example/items/screen1.png" alt="ItemCell component screenshot" width="300">
</p>


## Install

Install the package:

```bash
$ npm i react-native-common-item-cell --save
```

Install ``FontAwesome`` from the awesome Joel Oblador's ``react-native-vector-icons``: https://github.com/oblador/react-native-vector-icons#installation

## Usage

```javascript
import ItemCell from 'react-native-common-item-cell';
```

```javascript
<ItemCell>
  Item
</ItemCell>
```

```javascript
<ItemCell icon={{uri}}>
  Item
</ItemCell>
```

```javascript
<ItemCell title = {"Gender:"} showDisclosureIndicator={true}
onPress ={()=>console.log("pressme")}
onPressOut ={()=>console.log("pressme out")}>
  Man
</ItemCell>
```

## Prop API

| Prop | Type | Description |
|------|------|-------------|
|``showDisclosureIndicator`` | ``bool`` | Shows a small arrow at the right side of the cell. |
|``icon`` | ``{uri: string}`` object or ``require()`` | URI to render left icon with an URL for the image source or ``require`` for a local image source. |
|``children`` | ``string`` | The inner text to render. |
|``title`` | ``string`` | The left title text to render. |

## License

MIT
