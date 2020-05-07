<template>
<div class="categoryCreator categoryEntry">
        <form id="forCategory" class="inputArea centerMeXY">
            <img src="@/assets/cancel.svg" id="reset" @click="reset" alt="">
            <label for="">Category Name</label>
            <input type="text" id="categoryName" v-on:keyup.enter="addOrEdit('category')" class="bgTrasluscent">
            <label for="">Link to Logo</label>
            <input type="text" id="categoryLogo" v-on:keyup.enter="addOrEdit('category')" class="bgTrasluscent">
            <div class="inputBt" id="addBt" @click="createCategory">
            <p>Create Category</p>  
            </div>
            <div class="inputBt" id="saveEditBt" @click="saveEdit">
            <p>Edit Category</p>  
            </div>
        </form>


    <img src="@/assets/up.svg" @click="toCreateCategory" alt="" id="back">
      <div class="pBody centerMeXY" id="forAreas">
    <p class="ptitle centerMeX" id="cName">{{generated.category}}</p>
        <div class="list aCont">
        <form id="forAr" class="inputArea">
            <img src="@/assets/cancel.svg" id="reset" @click="reset" alt="">
            <label for="">Area Name</label>
            <input type="text" id="areaName" v-on:keyup.enter="addOrEdit('area')" class="bgTrasluscent">
            <label for="">Link to Logo</label>
            <input type="text" id="areaLogo" v-on:keyup.enter="addOrEdit('area')" class="bgTrasluscent">
            <div class="inputBt" id="addBt" @click="addArea">
            <p>Add Area</p>  
                <!-- <img src="@/assets/plus.svg" alt=""> -->
            </div>
            <div class="inputBt" id="saveEditBt" @click="saveEdit">
            <p>Edit Area</p>  
                <img src="@/assets/penn.svg" alt="">
            </div>
        </form> 
        <div class="itemsContainer listCont" id="listCont">
            <div :key="area.id" v-for="(area,index) in generated.areas" @click="showActions" class="item">
                <p>{{area.name}}</p>  
                <div class="itemActions" id="arActions">
                    <img src="@/assets/pen.svg" @click="toggleEditMode(index,studyPoint.point)" alt="" class="editIcon">
                    <img src="@/assets/trash.svg" @click="confirmDelete(index)" alt="" class="delIcon">
                </div>
            </div>
        </div>
      </div>

    </div>
</div>
</template>

<script>
export default {
    data(){return{
      generated:{
            category:'Video Editing',
            logo:'@/assets/logo.png',
            id:151515,
            areas:[
                {
                    name:'Premeir Pro',
                    id:1515515
                },
                {
                    name:'DaVinci Studio',
                    id:15115
                },
                {
                    name:'Hitfilm Express',
                    id:1518515
                },
            ],
  
        },
    }},
    methods:{
        toCreateCategory(){
            document.querySelector('.categoryCreator').classList.remove('areaEntry')
            document.querySelector('.categoryCreator').classList.add('categoryEntry')
        },
        toAreaCreate(){
            document.querySelector('.categoryCreator').classList.add('areaEntry')
            document.querySelector('.categoryCreator').classList.remove('categoryEntry')
        },
        reset(){
            const pointsContainer = document.querySelector('.pointsContainer')
            pointsContainer.classList.remove('editMode');
            if(pointsContainer.querySelector('.activeItem')){
                pointsContainer.querySelector('.activeItem').classList.add('item');   
                pointsContainer.querySelector('.activeItem').classList.remove('activeItem');
            }
            if(pointsContainer.classList.contains('showReset')){
                pointsContainer.classList.remove('showReset');
            }
            pointsContainer.querySelector('input').value = '';   
        },
        saveEdit(){
            // this.generated.studyPoints[this.targetPointIndex].point = document.querySelector('input').value;
            this.reset();
            document.querySelector('.confirmer').style.display = "none";
            const prompt = document.querySelector('#prompt');
                if(prompt){
                    document.querySelector('.notification').removeChild(prompt)
                }
            this.notify('<p id="saveSucess">Changes Saved</p>','#06AC61',1500)
        },
        addOrEdit(){
            const inEditMode = document.querySelector('.pointsContainer').classList.contains('editMode');
            if(inEditMode){
                this.saveEdit()
            }else{
                this.addArea()
            }
        },
        createCategory(){
            const categoryName = document.querySelector('#categoryName').value
            const categoryLogo = document.querySelector('#categoryLogo').value;
            if(categoryLogo && categoryName){
                this.generated.category = categoryName;
                this.generated.logo =categoryLogo;
                this.generated.id = Date.now();
                this.generated.areas = []    
                this.toAreaCreate()
            }
        },
        addArea(){
            const areaName = document.querySelector('#areaName');
            const areaLogo = document.querySelector('#areaLogo');
            if(areaName && areaLogo){
                const area = {
                    name:areaName.value,
                    logo:areaLogo.value,
                    id:Date.now()
                }
            this.generated.areas.unshift(area);
            this.clearInputs([areaLogo,areaName])
            }
        },
        toggleEditMode(index,point){
            // this.targetPointIndex = index;
            document.querySelector('.pointsContainer').classList.add('editMode')
            document.querySelector('#pointEntry').value = point
        },
        confirmDelete(index){
            // this.targetPointIndex = index;
            document.querySelector('.notification').style.background = 'orange'

            const prompt = document.querySelector('#prompt');
                if(prompt){
                    document.querySelector('.notification').removeChild(prompt)
                }
            document.querySelector('.confirmer').style.display = 'flex'
            this.showNotification()

        },
        showActions(e){
            const listCont = document.querySelector('.listCont')

            if(!e.currentTarget.classList.contains('activeItem')){
                if(listCont.querySelector('.activeItem') != null){
                    listCont.querySelector('.activeItem').classList.add('item')
                    listCont.querySelector('.activeItem').classList.remove('activeItem')
                    listCont.querySelector('.activeArea').classList.remove('activeArea')
                }
            }
            e.currentTarget.classList.toggle('activeItem')
            e.currentTarget.classList.toggle('activeArea')

            e.currentTarget.classList.toggle('item')
        },
        clearInputs(inputs){
            console.log(inputs);
            inputs.forEach(input => {
                input.value = ''
            });
        }
    }

}
</script>

<style scoped>
#forCategory{
    transition: 0.5s;
}
#forAreas{
    transition: 0.5s
}
.areaEntry #forAreas{
    top: 50%;
}
.areaEntry #forCategory{
    top: -100%;
}
.categoryEntry #forCategory{
    top: 50%;
}
.categoryEntry #forAreas{
    top: 200%;
}
.categoryEntry #back{
    opacity: 0;
}
.centerMeX{
    left: 50%;
    transform: translateX(-50%);
}
.centerMeXY{
    left: 50%;
    transform:translateX(-50%) translateY(-50%);
}
.categoryCreator{
    padding-top: 10px;
    position: relative;
    height: 100vh;
    overflow: hidden;
}
#cName{
    width: auto;
    padding: 3px 10px 5px 10px;
    position: absolute;
    top:0%;
    transform: translateX(-50%)  translateY(8px);
    z-index: 2;
    border-radius: 20px;
    color: white;
    background: linear-gradient(90deg,rgb(255, 0, 242),orange);
}
.aCont{
    position: relative;
    width: 100%;
    background: none;
    justify-content: space-between;
    align-items: center;
    margin-top: 50px;
}
#forCategory{
    position: absolute;
    width: 50%;
    margin: auto;
    display: flex;
    flex-direction: column;
    padding: 20px;
    justify-content: space-evenly;
    background: rgba(255, 255, 255, 0.082);
    padding-bottom: 10px;
}
#forAr{
    width: 40%;
    margin-left: 0px;
    display: flex;
    flex-direction: column;
    padding-top: 10px;
    justify-content: space-evenly;
    background: rgba(255, 255, 255, 0.082);
    padding: 10px;
    margin-bottom: 10px;
    margin-top: 0px !important;
}
#forAr .inputBt{
    margin-bottom: 0px;
}
form input{
    border-radius: 20px;
    margin: 10px;
    transition: 0.2s;
}
form .inputBt{
    border-radius: 2px;
    width: 100%;
    margin: auto;
    margin-bottom: 2px;
    margin-top: 10px;
    justify-content: space-evenly;
    transition: 0.2s;
}
form .inputBt:hover{
    text-transform: uppercase;
    letter-spacing: 2px;
    border-top-left-radius:20px;
    border-bottom-right-radius:20px;
}
form input:focus{
    border-radius: 2px;

}
form label{
    color: white;
    margin-left: 15px;
}
#forAreas{
    position: absolute;
    width: 50%;
    margin-top: -60px;
    top: 50%;
    max-height: 50vh;
    min-height: 200px;
    padding-top: 0px;
    background: rgba(255, 255, 255, 0.075);
    border-radius: 12px 12px 0px 0px;
}
::-webkit-scrollbar{
    display: none;
}
#forAreas .inputArea{
    margin-top: 20px;
}
#arActions{
    bottom: 0%;
    flex-direction: row;
    width: 100%;
    left: 50%;
    justify-content: space-around;
    transform: scale(0) translateX(-50%);
}
#arActions img{
    background: none;
}
.activeArea{
    margin-bottom: 40px;
}
.activeItem #arActions{
    transform: scale(1) translateY(100%) translateX(-50%);
    
}
#listCont{
    /* overflow-y: scroll; */
    margin-left:40px ;
    width: 500px;
    height: 100%;
    width: 40%;
    position: absolute;
    right: 10px;
    top:0px;
    margin-top: -5px;
    border: 1px solid rgba(255, 255, 255, 0.315);
    padding: 5px;
    border-radius: 10px 10px 0px 0px;
}

</style>