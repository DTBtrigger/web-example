# 记录一下学习 web 前端写的一些练习

## tips

- 获取输入框指值，使用`value`而不是`innerHTML`

- 获取表单元素：

  ```javaScript
      <form action="" name="form01">
      name:
      <input type="text" name="username" id="username" />
      <input type="submit" onclick="isempty()" />
      </form>
  ```

  ```javaScript
      let isempty = () => {

          let input = document.forms['form01']['username'].value
          if (input == '') {
          alert('姓名不能为空')
          }
      }
  ```

## problem and solution

- 需求，按下按钮后，小方块移动，直到大方块右下角停止

  ```javaScript
  let move = () => {
      let pos = 0
      let x = document.getElementById('small')
      let y = document.getElementById('small')
      let step = () => {
        pos = pos + 50
        x.style.left = pos + 'px'
        y.style.top = pos + 'px'
        console.log(`pos=${pos},id=${id}`)
      }
      let id = setInterval(step, 1000)
      let ans = 0
      if (pos === 350) {
        ans++
        clearInterval(id)
      }
      console.log('ans=' + ans)
    }
  ```

  **问题：** 小方块能移动，但是没有在大方块的右下角停止  
   **原因：**

  - 异步函数`setInterval`异步执行函数`step`，在`move`函数主线程执行完同步任务之后执行
  - 对`pos`的`if`判断在`move`中同步执行。即，在`setInterval`执行之前执行
  - 在后续执行`step`时，对`pos`的`if`判断不会再执行。即，对`pos`的`if`判断只在最开始执行了一次

  **解决：**
  将对`pos`的`if`判断放在`step`中

  ```javaScript
      let move = () => {
      let pos = 0
      let x = document.getElementById('small')

      let step = () => {
        if (pos === 350) {
          clearInterval(id)
        } else {
          pos++
          x.style.left = pos + 'px'
          x.style.top = pos + 'px'
        }
      }

      let id = setInterval(step, 5)
    }
  ```

- **问题：** 点击按钮时，没有输出对应的`value`值

  ```javaScript
  <button name="course" value="2" onclick="showCourse(this)">第2门课</button>

  let showCourse = (input) => {
    console.log(input.value)
    console.log('进入了')
  }

  ```

  **原因：** 不是没输出，而是输出了但页面被刷新，导致我没看见输出。  
  HTML 中`button`默认情况下会提交表单，导致页面刷新
  **解决：** 显式声明`button`类型：`type='button'`

- 回调**不等于**异步(暂时还没搞清楚，之后再说)
- `...args`和`...arguments`，在`js-practice10.html`中（之后再说）
