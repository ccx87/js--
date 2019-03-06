js如何实现数组扁平化


下面列举几种实现方式：
1. lodash/flatten, lodash/flattenDeep

   lodash链接：https://www.lodashjs.com/

1. 循环数组+递归

实现思路：循环数组，如果数据中还有数组的话，递归调用flatten扁平函数（利用for循环扁平），用concat连接，最终返回result;

    function flatten(arr){
 
        var result = [];

        for(var i = 0, len = arr.length; i < len; i++){

            if(Array.isArray(arr[i])){

                result = result.concat(flatten(arr[i]));

            }else{

                result.push(arr[i]);

            }

        }

        return result;
     
    }

    flatten(arr)   // [1,2,3,4]

2. 利用apply

可以使用apply的原因如下：

var arr = [1, [2, 3, [4]]];

[].concat.apply([],arr);

// [1,2,3,[4]]

实现思路：利用arr.some判断当数组中还有数组的话，递归调用flatten扁平函数(利用apply扁平), 用concat连接，最终返回arr;

    function flatten(arr){

       while(arr.some(item => Array.isArray(item))){

             arr =  [].concat.apply([],arr);

       }

       return arr;

    }

    flatten(arr)   // [1,2,3,4]

3. reduce方法

reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。 

能使用reduce原因如下：

      var flattened = arr.reduce(function(prev, cur){

          return prev.concat(cur)

      },[])

      console.log(flattened);

      //  [1,2,3,[4]]

实现思路：使用reduce, 当数组中还有数组的话，递归调用flatten扁平函数(利用reduce扁平), 用concat连接，最终返回arr.reduce的返回值;

      function flatten(arr){

          return arr.reduce(function(prev, cur){

              return prev.concat(Array.isArray(cur) ? flatten(cur) : cur)

          },[])

      }

      flatten(arr)   // [1,2,3,4]

4. es6 展开运算符

可以使用es6 展开运算符的原因如下：

var arr = [1, [2, 3, [4]]];

console.log(...arr);

// 1,[2,3,[4]]

实现思路：利用arr.some判断当数组中还有数组的话，递归调用flatten扁平函数(利用es6展开运算符扁平), 用concat连接，最终返回arr;

      function flatten(arr){

        while(arr.some(item => Array.isArray(item))){

            arr = [].concat(...arr);

        }

        return arr;

      }

      flatten(arr)   // [1,2,3,4]

5. toString方法（数组元素为数字） 

如果数组的元素是数字，那么我们可以考虑toString()方法，其他情况不适用。原因如下：

[1, [2, 3, [4]]].toString()

// "1,2,3,4"

实现思路：数组适用toString()方法后变成以逗号分割的字符串，然后map遍历数组把每一项再变回整数并返回map后的结果。

      function flatten(arr){

          return arr.toString().split(',').map(function(item){

               return parseInt(item);

           })

      }    

      flatten(arr)   // [1,2,3,4]
