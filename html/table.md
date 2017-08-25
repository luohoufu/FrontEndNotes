```
<table border="1" width="600" frame="hsides" rules="groups">
     //caption元素定义表格标题，只能对每个表格定义一个标题，
     //通常这个标题会被居中与表格之上，规定标题的对其方式使用css样式
     <caption>My Ultimate Table</caption>
     //colgroup用于对表格中的列进行组合，主要用于对全部列应用样式，这个标签很有用，
     <colgroup span="1" width="200"></colgroup>
     <colgroup span="3" width="400"></colgroup>


<thead>
     <tr>
          <td>cell 1-1</td>
          <td>cell 1-2</td>
          <td>cell 1-3</td>
          //rowspan属性规定单元格可横跨的列数
          <td rowspan="2">cell 1-4</td>
     </tr>
</thead>
<tfoot>   //写在前面 优先渲染
     <tr>
          <td>cell 4-1</td>
          <td>cell 4-2</td>
          <td>cell 4-3</td>
          <td>cell 4-4</td>
     </tr>
</tfoot>
<tbody>
     <tr>
          <td>cell 2-1</td>
          <td>cell 2-2</td>
          <td>cell 2-3</td>
          <td>cell 2-4</td>
     </tr>
     <tr>
          <td>cell 3-1</td>
          <td>cell 3-2</td>
          <td>cell 3-3</td>
          <td>cell 3-4</td>
     </tr>
</tbody>
</table>
```

## td标签

属性：

* colspan：规定单元格可横跨的列数
* rowspan：规定单元格可横跨的行数

## border-collapse

此css属性用来决定表格的边框是分开的还是合并的，在分隔模式下，

* collapse   用合并的边框绘制表格
* separate   用分隔的边框来绘制表格

## border-spacing

