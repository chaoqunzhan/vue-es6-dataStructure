<template>
  <div class="hello">
   <ul>Array before sort: {{array1}}</ul>
   <ul>Array after bubbleSort: {{array2}}</ul>
   <ul>Array after modify bubbleSort: {{array3}}</ul>
   <ul>Array after selectSort: {{array4}}</ul>
   <ul>Array after insertSort: {{array5}}</ul>
   <!-- <ul>Array after mergeSort: {{array6}}</ul> -->
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      array1: '',
      array2: '',
      array3: '',
      array4: '',
      array5: '',
      // array6: ''
    }
  },
  created:function(){
    const array = new Array(1,3,2,45,89,45,23,33,2,31,1,45);
    this.array1 = array;
    this.bubbleSort();
    this.modBubbleSort();
    this.selectSort();
    this.insertSort();
    // this.mergeSort();
  },
  methods: {
    swap(a,b,arr){
      let c = arr[a];
      arr[a] = arr[b];
      arr[b] = c;
    },

    bubbleSort(){
      let arr = this.array1;
      for (let i=0; i<arr.length; i++){
        for (let j=0; j<arr.length-1; j++){
          if (arr[j] > arr[j+1]){
            this.swap(j,j+1,arr);
          }
        }
      }
      this.array2 = arr;
    },

    modBubbleSort(){
      let arr = this.array1;
      for (let i=0; i<arr.length; i++){
        for (let j=0; j<arr.length-1-i; j++){
          if (arr[j] > arr[j+1]){
            this.swap(j,j+1,arr);
          }
        }
      }
      this.array3 = arr;
    },

    selectSort(){
      let arr = this.array1;
      let min;
      for(let i=0; i<arr.length; i++){
        min = i;
        for(let j=i; j<arr.length; j++){
          if(arr[min] > arr[j+1]){
            min = j+1;
          }
        }
        if(min!==i){
          this.swap(min,i,arr)
        }
      }
      this.array4 = arr;    
    },

    insertSort(){
      let arr = this.array1;
      for(let i=1; i<arr.length; i++){
        let temp = arr[i];
        let j = i;
        while(j>0 && arr[j-1]>temp){
          arr[j] = arr[j-1];
          j--;
        }
        arr[j] = temp;
      }
      this.array5 = arr;
    },

    // mergeSort(){
    //   let arr = this.array1;
    //   this.array6 = this.mergeSortRec(arr);
    // },

    // mergeSortRec(arr){
    //   if(arr.length===1){
    //     return arr;
    //   }
    //   let mid = arr.length/2;
    //   let left = array(0,mid);
    //   let right = array(mid,arr.length);
    //   return this.merge(mergeSortRec(left),mergeSortRec(right))
    // },

    // merge(left,right){
    //   let res = [];
    //   while(left.length>0 && right.length>0){
    //     let x = left.shift(), y = right.shift();
    //     if(x<=y){
    //       res.push(x); right.unshift(y);
    //     } else{
    //       res.push(y); left.unshift(x);
    //     }
    //   }
    //   if(left.length == 0){
    //     return res.concat(right);
    //   }else{
    //     return res.concat(left);
    //   }
    // }
  }
}
</script>


