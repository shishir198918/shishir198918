---
title: Vanila CSS
type: logs
prev: /
# next:
---

A simple demo page.

```css
body{
    height:100vh;
    width:100vw;
}
.header{
    width: 100%;
    height: 10%;
    background-color: rgb(80, 139, 190); /* Blue color */
    border: 2px solid #09070700;
    margin: 20px 20px 0px 20px;
}

.body{
    display:grid;
    grid-template-columns:25% 15% 35% 15% 25%;
    grid-template-rows: 25% 25% 25% 25%;
    margin: 20px;
    height: 90%;
    width:100%;
    background-color:coral
}
#path1
{
  width:100%;
  height:100%;
  grid-column:2/3;
  grid-row:1/5;
  background-color:brown;
  border: 2px solid #09070700;
  
}

#path2{
  width:15%;
  height:25%;
  grid-column-start:4;
  grid-row:1/5;
  background-color:brown;
  border: 2px solid #09070700;
  

}
```