<template>
<div class="pathCreator">
    <div class="notification">
        <div class="confirmer">
            <p>Continue to Delete?</p>
            <button @click="deleteStudyPoint">Yes</button>
            <button @click="hideNotification">No</button>
        </div>
    </div>
<div class="selector">
    <div class="list">
      <p class="title">Select Category</p>
      <div class="itemsContainer">
        <div :key="category.id" v-for="(category,index) in categories" @click="selectCategory($event,index,category.cName)" class="item">
            <p>{{category.cName}}</p>  
        </div>
      </div>
    </div>
    <div class="list areas" v-if="areas[0]">
      <p class="title">Select Area</p>
      <div class="itemsContainer">
        <div :key="area.id" v-for="(area,index) in areas" @click="selectArea($event,index,area.arName)" class="item">
            <p>{{area.arName}}</p>  
        </div>
      </div>
    </div>
    <div v-if="selectedArea" class="questions bgTrasluscent">
        <div class="title">{{selectedArea}}</div>
        <div class="qss">
            <div :key="question.question" v-for="question in questions" class="qsCont">
                <p class="qs">{{question.question}}  </p>
                <div class="answers">
                    <p class="ans" id="yes" @click="selectAnswer($event,'yes',question)">Yes</p>
                    <p class="ans" id="no" @click="selectAnswer($event,'no',question)">No</p>
                </div>
            </div>
            <p class="next" @click="saveProgress">Next</p>
        </div>
    </div>
</div>
<div class="pointsContainer">
    <img src="@/assets/up.svg" @click="backToSelection" alt="" id="back">
    <p class="ptitle">Path Creator</p>
      <div class="pBody">
        <div class="list sPoints">
      <p class="title">{{generated.area}}</p>
        <div class="inputArea">
            <img src="@/assets/cancel.svg" id="reset" @click="reset" alt="">
            <input type="text" id="pointEntry" v-on:keyup.enter="addOrEdit" class="bgTrasluscent">
            <div class="inputBt" id="addBt" @click="addPoint">
            <p>Add Study Point</p>  
                <img src="@/assets/plus.svg" alt="">
            </div>
            <div class="inputBt" id="saveEditBt" @click="saveEdit">
            <p>Edit Study Point</p>  
                <img src="@/assets/penn.svg" alt="">
            </div>
        </div>
        <div class="itemsContainer">
            <div :key="studyPoint.id" v-for="(studyPoint,index) in generated.studyPoints" @click="showActions" class="item">
                <p>{{studyPoint.point}}</p>  
                <div class="itemActions">
                    <img src="@/assets/triangle.svg" alt="" class="triIcon">
                    <img src="@/assets/pen.svg" @click="toggleEditMode(index,studyPoint.point)" alt="" class="editIcon">
                    <img src="@/assets/trash.svg" @click="confirmDelete(index)" alt="" class="delIcon">
                </div>
            </div>
        </div>
      </div>

    </div>
</div>
</div>
</template>

<script>
// import listView from '@/components/list.vu e'

export default {
    methods:{
        notify(notification,color,timeout){
            document.querySelector('.notification').insertAdjacentHTML('afterbegin',`${notification}`)
            document.querySelector('.notification').style.background = `${color}`;
            this.showNotification(timeout);    
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
        showActions(e){
            const pointsContainer = document.querySelector('.pointsContainer')

            if(pointsContainer.querySelector('.activeItem')){
                pointsContainer.querySelector('.activeItem').classList.add('item')
                pointsContainer.querySelector('.activeItem').classList.remove('activeItem')
            }
            e.currentTarget.classList.toggle('activeItem')
            e.currentTarget.classList.toggle('item')
        },
        addOrEdit(){
            const inEditMode = document.querySelector('.pointsContainer').classList.contains('editMode');
            if(inEditMode){
                this.saveEdit()
            }else{
                this.addPoint()
            }
        },
        addPoint(){
            const entry = document.querySelector('#pointEntry').value;
            const studyPoint = {
                point:entry,
                id:Date.now()
            }
            if(studyPoint.point){
                this.generated.studyPoints.unshift(studyPoint)
                this.reset();
            }
        },
        toggleEditMode(index,point){
            this.targetPointIndex = index;
            document.querySelector('.pointsContainer').classList.add('editMode')
            document.querySelector('#pointEntry').value = point
        },
        saveEdit(){
            this.generated.studyPoints[this.targetPointIndex].point = document.querySelector('input').value;
            this.reset();
            document.querySelector('.confirmer').style.display = "none";
            const prompt = document.querySelector('#prompt');
                if(prompt){
                    document.querySelector('.notification').removeChild(prompt)
                }
            this.notify('<p id="saveSucess">Changes Saved</p>','#06AC61',1500)
        },
        confirmDelete(index){
            this.targetPointIndex = index;
            document.querySelector('.notification').style.background = 'orange'

            const prompt = document.querySelector('#prompt');
                if(prompt){
                    document.querySelector('.notification').removeChild(prompt)
                }
            document.querySelector('.confirmer').style.display = 'flex'
            this.showNotification()

        },
        deleteStudyPoint(){
            this.generated.studyPoints.splice(this.targetPointIndex,1);
            this.hideNotification()
        },
        selectCategory(e,index,category){
            if(document.querySelector('.activeItem')){
                document.querySelector('.activeItem').classList.add('item')
                document.querySelector('.activeItem').classList.remove('activeItem')
            }
            e.currentTarget.classList.toggle('activeItem')
            e.currentTarget.classList.toggle('item');
            const areas = document.querySelector('.areas');
            if(areas){
            const prevArea =areas.querySelector('.activeItem');
            if(prevArea){
                prevArea.classList.remove('activeItem');
                prevArea.classList.add('item');
            }
            }
            this.generated.category = category;
            this.showAreas(index)
            this.removePreviousValues();

        },
        showAreas(categoryIndex){
            this.areas = [...this.categories[categoryIndex].areas]
        },
        selectArea(e,index,area){
            const prevSelect = e.currentTarget.parentElement.querySelector('.activeItem');
            if(prevSelect){
                prevSelect.classList.add('item')
                prevSelect.classList.remove('activeItem')
            }
            e.currentTarget.classList.toggle('activeItem');
            e.currentTarget.classList.toggle('item');
            this.generated.area = area;
            this.selectedArea = area;
            //REMOVE PREVIOUS VALUES
            this.removePreviousValues();
        },
        removePreviousValues(){
            if(document.querySelector('.selected')){
                const answered = document.querySelectorAll('.selected');
                answered.forEach(answer => {
                    answer.classList.remove('selected')
                });
                this.generated.hasIntros = null;
                this.generated.hasProjects = null;
                this.generated.hasFramework = null;
            }
            this.generated.studyPoints = [];
        },
        selectAnswer(e,choice,question){
            if(choice=='yes'){
                question.answer = true
                const alternative = e.currentTarget.parentElement.querySelector('#no');
                alternative.classList.remove('selected')
                this.recordChoice(question)
            }else{
                question.answer = false
                const alternative = e.currentTarget.parentElement.querySelector('#yes');
                alternative.classList.remove('selected')
                this.recordChoice(question)
            }
            e.currentTarget.classList.add('selected')
        },
        recordChoice(question){
            if(question.qLabel == 'hasIntros'){
                this.generated.hasIntros = question.answer
            }else if(question.qLabel == 'hasProjects'){
                this.generated.hasProjects = question.answer
            }else{
                this.generated.hasFramework = question.answer
            }
        },
        saveProgress(){
            if(this.generated.hasIntros == null || this.generated.hasProjects == null || this.generated.hasFramework == null){
                const prompt = document.querySelector('#prompt');
                if(prompt){
                    document.querySelector('.notification').removeChild(prompt)
                }
                this.notify('<p id="prompt">Please answer All questions</p>','rgb(255, 0, 106)',1500)


                document.querySelector('.confirmer').style.display = 'none';
            }else{
                document.querySelector('.pointsContainer').style.top="0%";
                document.querySelector('.selector').style.top="-100%";

            }
        },
        backToSelection(){
            document.querySelector('.pointsContainer').style.top="100%";
            document.querySelector('.selector').style.top="0%"; 
        },
        showNotification(timeout){
            document.querySelector('.notification').classList.add('notify');
            if(timeout){
                setTimeout(()=>{
                    document.querySelector('.notification').classList.remove('notify');
                    document.querySelector('.confirmer').style.display = "none"
                    const saveSucess = document.querySelector('#saveSucess');
                    const prompt = document.querySelector('#saveSucess');
                    if(saveSucess){
                        document.querySelector('.notification').removeChild(saveSucess)
                        document.querySelector('.confirmer').style.display = 'flex'
                    }
                    if(prompt){
                        document.querySelector('.notification').removeChild(prompt)
                        document.querySelector('.confirmer').style.display = 'flex'
                    }
                },timeout)
            }
        },
        hideNotification(){
            document.querySelector('.notification').classList.remove('notify');
            // this.reset()
        }
    },
components:{
    // listView
},
created(){
    setTimeout(()=>{
        document.querySelector('input').addEventListener('keyup',(e)=>{
            if(e.key != 'Enter'){
                document.querySelector('.pointsContainer').classList.add('showReset');
            }
        })
    },0)
},
data(){
    return{
        targetPointIndex:null,
        generated:{
            category:null,
            area:null,
            hasIntros:null,
            hasProjects:null,
            hasFramework:null,
            studyPoints:[
    
            ]
        },
        questions:[
            {
                question:'Has Introductory type videos',
                qLabel:'hasIntros',
                answer:null
            },
            {
                question:'Has Project type videos',
                qLabel:'hasProjects',
                answer:null
            },
            {
                question:'Has Framework',
                qLabel:'hasFramework',
                answer:null
            },
        ],
        areas:[],
        selectedArea:null,
        categories:[
            {
                id:5565,
                cName:"Video Editing",
                areas:[
                    {
                        id:5653,
                        arName:"Premier pro"
                    },
                    {
                        id:5853,
                        arName:"Hitfilm Express"
                    },
                    {
                        id:59953,
                        arName:"Da'Vinci Resolve"
                    },
                    {
                        id:56535,
                        arName:"Sony Vegas Pro"
                    },
                ]
            },
            {
                id:559565,
                cName:"Programming",
                areas:[
                    {
                        id:5653,
                        arName:"Python"
                    },
                    {
                        id:5853,
                        arName:"Javascript"
                    },
                    {
                        id:59953,
                        arName:"Java"
                    },
                    {
                        id:56535,
                        arName:"C#"
                    },
                ]
            },
        ]
    }
}
}
</script>


<style>
.pathCreator{
    position: relative;
    height: 100vh;
    width: 100%;
    overflow: hidden;
}
.selector{
    display: flex;
    justify-content:space-around;
    align-items: center;
    transition: 0.2s;
    position: absolute;
    height: 100vh;
    width: 100%;
    top:0%;
    transition: 0.4s;

    /* display: none; */
}
.bgTrasluscent{
    background: rgba(255, 255, 255, 0.082);

}
.list{
    background: rgba(255, 255, 255, 0.158);
    width: 250px;
    color: white;
    margin-top: 50px;
    margin-left: 10px;
    border-radius: 5px;
    padding-bottom: 5px;
}
.title{
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
.questions{
}
.qss{
    color: white;
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    row-gap: 30px;
    padding: 5px;
    padding-right: 30px;
}
.qsCont{
    width: 100%;
    margin: 10px;
    text-align: center;
}
.qs{
    margin-bottom: 10px;
}
.answers{
    display: flex;
    justify-content:space-between;
}
.ans{
    background: rgba(255, 255, 255, 0.158);
    padding: 5px;
    padding-right: 15px;
    padding-left: 15px;
    border-radius: 30px;
    transition: 0.2s;
}
.ans:hover{
    border-radius: 0;
    border-top-left-radius:10px;
    border-bottom-right-radius:10px;
    cursor: pointer;
    background: white;
    color: black;
}
.next{
    background: white;
    color: black;
    padding: 5px;
    padding-left: 20px;
    padding-right: 20px;
    text-align: center;
    margin-right: -25px;
    border-radius: 2px;
    transition: 0.2s;
    font-size: 1.1em;
    text-transform: uppercase;
    margin-bottom: ;
}
.next:hover{
    cursor: pointer;
    border-radius: 30px;
    border-top-right-radius: 0px;
    border-bottom-left-radius: 0px;
    color: white;
    background: linear-gradient(90deg,rgb(255, 0, 242),rgb(162, 0, 255));
}
.selected{
    background: linear-gradient(90deg,rgb(255, 0, 242),orange);
}
.selected:hover{
    cursor: default;
    border-radius: 20px;
    background: linear-gradient(90deg,rgb(255, 0, 242),orange);
    color: white;
}
.notification{
    position: absolute;
    top:-100%;
    background: rgb(255, 123, 0);
    padding: 10px;
    color: white;
    width: 100%;
    z-index: 11;
    text-align: center;
    font-size: 1.2em;
}
.notify{
    top:0%;
}
.confirmer{
    display: flex;
    justify-content: center;
    align-items: center;
}
.confirmer button{
    outline: none;
    border: none;
    background: white;
    border-radius: 2px;
    padding: 5px;
    margin: 5px;
    transition: 0.2s;
    width: 50px;
    border:1.5px solid transparent;
}
.confirmer button:hover{
    border:1.5px solid white;
    font-weight: bold;
    cursor: pointer;
    border-top-left-radius:10px;
    border-bottom-right-radius:10px;
    color: white;
    background: linear-gradient(90deg,rgb(255, 0, 242),rgb(225, 0, 255));
}
.pointsContainer{
    padding: 5px;
    position: absolute;
    top: 100%;
    left:50%;
    transform: translateX(-50%);
    width: 100%;    
    transition: 0.4s;
}
.pBody{
    display: flex;
    align-items: center;
    justify-content: center;
}
.ptitle{
    background: white;
    width: 20%;
    margin: auto;
    text-align: center;
    padding: 5px;
    font-size: 1.2em;
    border-radius: 30px;
}
.ptitle:hover{
    cursor: default;
}
.sPoints{
    width: 60%;
}
#reset{
    width: 30px;
    position: absolute;
    right: 0px;
    transform: scale(0);
    border-radius: 100%;
    transition: 0.2s;
}
#reset:hover{
    background: black;
    cursor: pointer;
}
.inputArea{
    position: relative;
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
    margin-bottom: 10px;
    padding: 5px;
    border-radius: 15px;
}
input{
    border: none;
    outline: none;
    padding: 5px;
    color: white;
}
.inputArea input{
    flex-grow: 3;
    font-size: 1.2em;
    padding-left:10px;
    border-top-left-radius: 15px;
    border-bottom-left-radius: 15px;

}
.inputBt{
    background: white;
    color: black;
    display: flex;
    flex-grow: 0;
    align-items: center;
    justify-content: space-between;
    padding: 5px;
    border-top-right-radius: 15px;
    border-bottom-right-radius: 15px;
}
.inputBt:hover{
    cursor: pointer;
    color: white;
    background: linear-gradient(90deg,rgb(255, 0, 242),rgb(162, 0, 255));
}
.inputBt:hover img{
    transform: rotate(90deg) scale(1);
}
#saveEditBt:hover img{
    transform: rotate(-45deg) scale(1);
}
.inputBt img{
    margin-left: 5px;
    width: 20px;
    transition: 0.2s ease-in-out;
}
#saveEditBt{
    display: none;
}
.editMode #addBt{
    display: none;
}
.editMode #saveEditBt{
    display: flex;
}
.editMode #reset{
    transform: scale(1);
    right:-35px;
}
.showReset #reset{
    transform: scale(1);
    right:-35px;
}
#back{
    position: absolute;
    right: 10px;
    top:10px;
    width: 50px;
}
#back:hover{
    animation: hop;
    animation-duration: 0.8s;
    cursor: pointer;
    animation-iteration-count: infinite;
    animation-timing-function: ease-in-out;
}
    @keyframes hop {
        0%{
            top: 20px;
        }
        50%{
            top: 0px;
        }
        100%{
            top: 20px;
        }
    }
</style>