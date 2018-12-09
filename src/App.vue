<template>
    <div id="app">
        <Flashcard
            @questionChanged="changeQuestion"
            :questionText="questionText"
            :answers="answers"
            :isFinished="isFinished">
        </Flashcard>

        <ProgressBar
            :currentValue="activeQuestion"
            :maximumValue="numberOfQuestions">
        </ProgressBar>
    </div>
</template>

<script>
import Flashcard from './components/Flashcard.vue';
import ProgressBar from './components/ProgressBar.vue';
import correctSound from './sounds/correct_sound.mp3';
import incorrectSound from './sounds/incorrect_sound.mp3';
import resultsSound from './sounds/results_sound.mp3';

export default {
    data() {
        return {
            data: '',
            questionText: '',
            answers: '',
            activeQuestion: 0,
            numberOfQuestions: 0,
            playerPoints: 0,
            isFinished: false,
            correctSound: correctSound,
            incorrectSound: incorrectSound,
            resultsSound: resultsSound
        }
    },

    created() {
        fetch('https://gist.githubusercontent.com/vergilius/6d869a7448e405cb52d782120b77b82c/raw/e75dc7c19b918a9f0f5684595899dba2e5ad4f43/history-flashcards.json')
			.then(data => data.json())
			.then(data => this.data = data)
			.then(data => this.setData(data))
			.catch(error => console.log(error))
    },

    watch: {
        isFinished() {
            this.activeQuestion = this.numberOfQuestions;
            this.playSound(this.resultsSound);
        }
    },

    methods: {
        setData(data) {
            this.questionText = data[this.activeQuestion].question;
            this.answers = data[this.activeQuestion].answers;
            this.numberOfQuestions = data.length;
        },

        changeQuestion(isCorrect) {
            if (this.activeQuestion + 1 < this.numberOfQuestions) {
                this.activeQuestion++;
                this.setData(this.data);

                if (isCorrect) {
                    this.playerPoints++;
                    this.playSound(this.correctSound);
                } else {
                    this.playSound(this.incorrectSound);
                }
            } else {
                this.isFinished = true;
                this.questionText = `Your score: ${this.playerPoints} / ${this.numberOfQuestions}`;
            }
        },

        playSound (sound) {
            if(sound) {
                const audio = new Audio(sound);
                audio.play();
            }
        }
    },

    components: {
        Flashcard,
        ProgressBar
    }
}
</script>

<style lang="scss">
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 95vh;
}
</style>
