<script>

// state 0 = empty
// state 1 = red
// state 2 = black
// state 3 = posibble step
var bordArray = [
[0,1,0,1,0,1,0,1],
[1,0,1,0,1,0,1,0],
[0,1,0,1,0,1,0,1],
[0,0,0,0,0,0,0,0],
[0,0,0,0,0,0,0,0],
[2,0,2,0,2,0,2,0],
[0,2,0,2,0,2,0,2],
[2,0,2,0,2,0,2,0],
]

var endX
var endY
var startX
var startY


var handleDragEnter = (x,y)=>{
  endX= x
  endY = y
}

var handleDragStart = (x,y)=>{
  startX = x
  startY = y
  checkPosibilities()
}
  var hitRight = false
  var hitLeft = false
  var playerDirection
 
var checkPosibilities = () => {
 // check if possibilities are empty
  // check playing direction
  playerDirection = bordArray[startX][startY] == 1 ? 1 : -1 
  // check if empty right
  if(bordArray[startX+playerDirection][startY+1] !== 0){
    // check if stone is not same as current player
    if( bordArray[startX+playerDirection][startY+1] !== bordArray[startX][startY]){
      console.log('steen rechts')
      // check if empty after stone
     if(bordArray[startX+(playerDirection*2)][startY+2] == 0 ) { 
      console.log("slaan mogelijk rechts")
        // set state of box to 3
        bordArray[startX+(playerDirection*2)][startY+2] = 3
        hitRight = true
      }
    }
    }else {
      bordArray[startX+playerDirection][startY+1] = 3
      hitRight = false
   }

  // same as above but then for left direction
  if(bordArray[startX+playerDirection][startY-1] !== 0){
    if( bordArray[startX+playerDirection][startY-1] !== bordArray[startX][startY]){
      console.log('steen links')
      // check if empty after stone
     if(bordArray[startX+(playerDirection*2)][startY-2] == 0 ) { 
      console.log("slaan mogelijk links")
        bordArray[startX+(playerDirection*2)][startY-2] = 3   
        // if left hit possible, disable right step
        if(bordArray[startX+playerDirection][startY+1] == 3)bordArray[startX+playerDirection][startY+1] = 0
        hitLeft = true
 
      }
    }
   }else if(!hitRight) {
      bordArray[startX+playerDirection][startY-1] = 3
      hitLeft = false
   }



}

var handleDragEnd = ()=>{
  console.log("end"+endX+endY)
  console.log("start"+startX+startY)
  // check if posibility
  if(bordArray[endX][endY] !== 3) return
 
  // remove stone if hit
  if(hitRight) bordArray[startX+playerDirection][startY+1] = 0
  if(hitLeft) bordArray[startX+playerDirection][startY-1] = 0


  // remove possibilities after move
  for (let index = 0; index < bordArray.length; index++) {
      for (let index2 = 0; index2 < bordArray[index].length; index2++) {
        if(bordArray[index][index2] == 3) bordArray[index][index2] = 0 
      }   
  }



  // fill new field
  bordArray[endX][endY] = bordArray[startX][startY]
   // empty last field
  bordArray[startX][startY] = 0
  


}


</script>
<table>
{#each bordArray as row,x}
  <tr>
      {#each row as item,y}
         <td 
       >
          <div class="block {item == 3 ? 'step' : '' }" 
            on:dragenter={()=>{handleDragEnter(x,y)}} 
          >
          {#if item == 1}
              <div class="red" draggable="true"
                on:dragstart={()=>{handleDragStart(x,y)}}
            		on:dragend={handleDragEnd}
              ></div>
          {/if}
          {#if item == 2}
              <div class="black" draggable="true"
                on:dragstart={()=>{handleDragStart(x,y)}}
            		on:dragend={handleDragEnd}>
               </div>
          {/if}
              </div>
        </td> 
        
      {/each}
    
  </tr>
  {/each}
  </table>



<style>

  .block {
    width:50px;
    height:50px;
    border: 2px solid black;
  }

  tr:nth-child(odd) td:nth-child(even){
    background-color: grey;
  }

  tr:nth-child(even) td:nth-child(odd){
    background-color: grey;
  }


  .red{
    background-color: red;
    width:40px;
    height:40px;
    border-radius: 25px;
    margin: 0 auto;
  }

  .black{
    background-color:black;
    width:40px;
    height:40px;
    border-radius: 25px;
    margin: 0 auto;
  }

  .step{
    background-color:green;
  }


</style>
