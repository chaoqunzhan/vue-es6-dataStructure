var lines = new Array();
while(line=readline()){
    lines.push(line);
}
let mn = lines[0].split(' ')
let Str = lines[1].split(' ')[0];
let n = parseInt(mn[0]);
let m = parseInt(mn[1]);

var aIndex = new Array(), 
    bIndex = new Array();
    aIndex.push(-1); 
    bIndex.push(-1);  

for (var i=0; i<Str.length; i++){
    if(Str[i]=='a'){
        aIndex.push(i)
    }else{
        bIndex.push(i)
    }
}
aIndex.push(n); 
bIndex.push(n); 

//console.log(aIndex);
//console.log(bIndex);

var bMax = 0;

//bMax = maxLength(bMax, bIndex);
for (var i=m+1; i<bIndex.length; i++){
     var bDiff = bIndex[i]-1-bIndex[i-m-1]
     bMax<bDiff? bMax=bDiff:bMax=bMax;
}
//console.log(bMax);


var aMax = 0;

//bMax = maxLength(bMax, bIndex);
for (var i=m+1; i<aIndex.length; i++){
     var aDiff = aIndex[i]-1-aIndex[i-m-1]
     aMax<aDiff? aMax=aDiff:aMax=aMax;
}
//console.log(aMax);

aMax<bMax?console.log(bMax):console.log(aMax);



//function maxLength(Max,Index){
   // for (var i=m+1; i<Index.length; i++){
      //  var Diff = Index[i]-1-Index[i-m-1]
      //  Max<Diff? Max=Diff:Max=Max;
   // }
//}
