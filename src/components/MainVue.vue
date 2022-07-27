<template>
    <div class="main-wrapper">
    <div class="score">
    <p>Points : <span> {{totalScore}}</span></p>
    <p>Remaining Attempts : <span>{{totalAttempts}}</span></p>
    </div>
    <div v-if="gameContinue" class="animate__animated animate__slideInRight">
    <h2>Guess the Word : <span>{{randomWord}}</span></h2>
    <div class="result" v-if="checkAnswerCalled">
        <div class="correct" v-if="correctAnswer">
         <p>Your Answer is Correct</p>
        </div>
        <div class="false" v-if="!correctAnswer">
            <p>Your Answer is false</p>
        </div>
        
    </div>
    <div class="guess-wrapper">
        <input type="text" class="guess">
        <button @click="checkAnswer" type="submit" class="submit" value="Guess">Guess</button>
        <div>
              <button @click="moveNext">Pass Left {{totalPass}}</button>
        </div>
    </div>
        
    </div>
    <div v-else class="guess-wrapper animate__animated animate__slideInLeft">
        <h2>Game Completed</h2>
        <button @click="startAgain" class="secondary-btn">Play Again</button>
    </div>

    </div>
</template>
<script>
export default {
    data(){
        return {
            randomWord:'',
            allWords:[
                'orange',
                'pen',
                'desk',
                'butterfly',
                'chair',
                'plastic',
                'mirror',
                'refrigrator',
                'justice'
            ],
            // Virtual array of words
            virtualWords:[],
            newWord:'',
            gameContinue:true,
            // Sepreate the guessed words
            answeredWord:[],
            totalAttempts:3,
            totalScore:0,
            totalPass:3,
            correctAnswer:false,
            checkAnswerCalled:false
         
        }
    },
    methods:{
        // Pushing words to virtual array of words
        assignWords(){
        this.allWords.forEach(word=>{
        this.virtualWords.push(word)
       })
        },
        getRandomWord(){
            if(this.virtualWords.length>0){
                // Generating a random number to select random number from array
                let n= Math.floor(Math.random()*this.virtualWords.length)
                // Randomly selected Word 
                this.newWord=this.virtualWords[n]
                // Removing the selected word from array so that game cannot be repeated
                this.virtualWords.splice(n,1)
                // Splitting the new word into an array of letters so that it can bhe scrambled 
                this.randomWord=this.scramble(this.newWord.split(""))
            }else{
                this.gameContinue=false
            }
        },
        // Function to Scramble the words
        scramble(array){
            // Sorting the letters
            let trick=array.sort()
            // Reversed the position of letters
            trick=trick.reverse()
            // Joining words again as string
            return trick.join("")

        },
        // Get a new word after guess
        replay(){
                  document.getElementsByClassName('guess')[0].value=""
                    this.getRandomWord()
        },
        // Function to check answer
        checkAnswer(){
            
            let answer=document.getElementsByClassName('guess')[0].value
            if(!answer){
               alert('Write Something ')
               this.checkAnswerCalled=false
            }else if(answer.length!=0 & answer.toUpperCase()!=this.newWord.toUpperCase()){
                this.checkAnswerCalled=true
                // alert('Wrong Answer')
                this.correctAnswer=false;
            //If condition to check for totalAttempts 
            if(this.totalAttempts>=2){
                  this.totalAttempts--
              }else{
                 this.totalAttempts--
                this.gameContinue=false
              }
 
            }else{
                // alert('Correct Answer')
                this.checkAnswerCalled=true
               this.correctAnswer=true;
                setTimeout(
                    this.replay()
                ,3000)
        // Increment in scores
                this.totalScore+=5
            }
        },
        // Move to next pass
        moveNext(){
            if(this.totalPass>0){
                this.totalPass--
                this.replay()
            }else{
                alert('No Pass Left')
            }
        },
        // Function to play game again
        startAgain(){
            this.assignWords()
            this.gameContinue=true
            this.totalScore=0
            this.totalPass=3
            this.totalAttempts=3
           

        }
    },
    created(){
        this.assignWords()
        this.getRandomWord()
        window.addEventListener('keydown', (e) => {
      if (e.key == 'Enter') {
          let answer=document.getElementsByClassName('guess')[0].value
        if(answer.length>0){
            this.checkAnswer()
        }
      }
    });
        
    }

}
</script>
<style>
.guess-wrapper{
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.main-wrapper{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top:40px;
}
.score{
    width: 600px;
    display:flex;
    justify-content: space-around;
    background-color: #292929;
    padding:30px 20px;
    margin-bottom: 30px;
}
.score p{
    font-size:1.8em;
    font-family: 'Permanent Marker', cursive;
    
}
.result>div{
    width:400px;
    background: #292929;
    padding: 10px 20px;
    margin: 0 auto;
    margin-top:5px;
}
.result p{
    font-size:1.2em

}
.result .correct p{
    color:green;
}
.result .false p{
    color: red;
}
.main-wrapper span{
    color: #ffaf02;
    text-transform: uppercase;
}
.main-wrapper h2{
    font-size: 2.8em;
}
.guess-wrapper{
    margin-top: 40px;
}
.guess-wrapper .guess{
   width:400px;
   padding: 10px 20px;
   border-radius: 30px;
   border: none;
   font-size: 1.4em;
   margin-bottom: 30px;
}
input:focus{
    outline: none;
}

.main-wrapper button{
    padding:10px 30px;
    background: transparent;
    border: 2px solid #ffaf02;
    border-radius: 30px;
    cursor: pointer;
    color:#ffffff;
    font-size: 1.2rem;
    transition: all .5s;
}
.main-wrapper button:hover{
  
    background: #ffaf02;
    border: 2px solid #ffaf02;
   
    color:#ffffff;
  
}
.submit{
    margin-bottom: 30px;
}

.secondary-btn{
    margin-top: 30px
}
@media only screen and (max-width: 600px) {
 .score{
    width: 350px;
    

}
.guess-wrapper .guess{
   width:350px;
   padding: 10px 20px;
   border-radius: 30px;
  
}

}
</style>