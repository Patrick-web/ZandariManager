<template>
  <div class="list">
          <div @click="showActions($event)" class="item">
            <p>{{item.name}}</p>  
            <div class="itemActions">
                <img src="@/assets/triangle.svg" alt="" class="triIcon">
                <img src="@/assets/pen.svg" @click="editItem()" alt="" class="editIcon">
                <img src="@/assets/trash.svg" @click="deleteItem()" alt="" class="delIcon">
            </div>
          </div>

  </div>
</template>

<script>
export default {
    methods:{
        showActions(e){
            if(document.querySelector('.activeItem')){
                document.querySelector('.activeItem').classList.add('item')
                document.querySelector('.activeItem').classList.remove('activeItem')
            }
            e.currentTarget.classList.toggle('activeItem')
            e.currentTarget.classList.toggle('item')
        }
    }
}
</script>

<style>
.list{
    background: rgba(255, 255, 255, 0.158);
    width: 250px;
    color: white;
    margin-top: 50px;
    margin-left: 10px;
    border-radius: 5px;
    padding-bottom: 5px;
}
.listTitle{
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    background: rgb(255, 255, 255);
    padding: 8px;
    text-align: center;
    text-transform: uppercase;
    color: black;
}
.itemsContainer{
    margin-top: 3px;
}
.item{
    background: rgba(255, 255, 255, 0.116);
    color: rgb(255, 255, 255);
    padding: 5px;
    margin-bottom: 2px;
    font-size: 1.1em;
    transition: 0.2s;
    position: relative;
}
.item:hover{
    background: white;
    color: black;
    cursor: pointer;
    margin-right: 5px;
    margin-left: 5px;
    border-top-left-radius:10px;
    border-bottom-right-radius:10px;
    margin-bottom: 5px;
    margin-top: 5px;
}
.activeItem{
    background: linear-gradient(90deg,rgb(255, 0, 242),orange);
    color: white;
    padding: 5px;
    margin-bottom: 2px;
    font-size: 1.1em;
    position: relative;
}
.activeItem:hover{
    cursor: default;
}
.activeItem .itemActions{
    transform: scale(1) translateX(0%);
}
.activeItem img:hover{
    transform: scale(1.1);
    border-radius: 10px;
    cursor: pointer;
}
.itemActions{
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    right: -47px;
    flex-direction: column;
    z-index: 2;
    top:-5px;
    transform: scale(0) translateX(-600%);
    transform-origin: left;
    transition: 0.2s;
}
.editIcon,.delIcon{
    background: rgba(255, 255, 255, 0.199);
    width: 40px;
    margin-right: -15px;
    margin-left: -15px;
    padding: 5px;
    transition: 0.2s;
    /* transition-delay: 0.2s; */
}
.editIcon{
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
}
.delIcon{
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}
.triIcon{
    position: absolute;
    width: 22px;
    top:5px;
    right:25px;
}
</style>