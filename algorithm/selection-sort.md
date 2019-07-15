# 선택 정렬

```javascript
const selectionSort = ((arr) =>{
    for(let i = 0; i < arr.length; i++) {
        for(let j = i+1; j < arr.length; j++) {
            if(arr[i] > arr[j]) {
                let temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
});

let arr = [12,56,35,93,45,62,56,8];
console.log(selectionSort(arr));
```

