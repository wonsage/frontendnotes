#  JS 面试题：代码

1. ##### Vue/React 中为什么要在列表组件中写 key？  

   Vue 中，v-for 默认会尝试原地修改元素而非重新排序，没有 key 时，状态绑定的是位置。有 key 时，状态则根据 key 绑定到了对应的数组元素上。另外，也不能以不固定的 index 作为 key 值，key 值需要确切地指向某一唯一元素，比如 id。

2. ##### `['1', '2', '3'].map(parseInt)` what & why ?  

   一个数组调用 map 函数，回调函数为 parseInt  
   map 函数用于使用数组生成一个新数组，每个数组元素都要经过map 的回调处理后返回。map 的回调有三个参数：currentValue、[index]、[array]。  
   parseInt 的参数则有：string、[radix]。radix 为 2~36 的进制数。以 parseInt 作为回调，则元素的 index 作为 radix 传入，因此：  
   `parseInt('1', 0) = 1` radix 为0，且字符串不以0x或0开头，则默认10进制；  
   `parseInt('2', 1) = NaN` radix 为1，不是2~36之间的数，返回 NaN；  
   `parseInt('3', 2) = NaN` radix 为2，但3不是有效的二进制数，返回NaN

3. ```JS
   let a = {n:1}
   let b = a
   a.x = a = {n:2}
   console.log(a.x)
   console.log(b.x)
   ```

   undefined  
   {n:2}

4. 实现一个 sleep 函数，参数为毫秒，意为等待若干毫秒，可从 Promise、Generator、Async/Await 等角度实现。  

   ```js
   function sleep(ms) {
   	return new Promise(resolve=>{
       setTimeout(resolve, ms)
     })
   }
   
   (async function () {
     console.log(a)
     await sleep(2000)
     console.log(b)
   })()
   ```

5. 手写函数实现数字数组扁平化并去重，最终得到一个升序并不重复的数组。例如：`[[1,2,3], [3,4,5,5], [6,7,8,9,[11, 12, [12,13,14]]], 10]`  

   ```js
   let arr = [ [1,2,3], [3,4,5,5], [6,7,8,9,[11,12,[12,13,14]]], 10]
   let res = []
   let obj = {}
   function unfold (arr) {
     for (let i=0; i<arr.length; i++) {
       if (arr[i] instanceof Array) unfold (arr[i])
       else {
         if (!obj[arr[i]]) {
           res.push(arr[i])
           obj[arr[i]] = true
         }
       }
     }
   }
   unfold(arr)
   for (let i=1; i<res.length; i++) {
     for (let j=i; j>0; j--) {
       if (res[j] < res[j-1]) {
         let tmp = res[j]
         res[j] = res[j-1]
         res[j-1] = tmp
       }
     }
   }
   console.log(res)
   ```

6. 

