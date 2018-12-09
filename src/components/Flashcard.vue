<template>
    <div class="c-flashcard">
        <div class="c-flashcard__text-wrapper c-flashcard__text-wrapper--text-center">
            <p class="c-flashcard__text">{{ questionText }}</p>
        </div>
        <div v-if="!isFinished" class="c-flashcard__buttons-wrapper">
            <button
                class="c-flashcard__btn"
                v-for="answer in shuffleArray(answers)"
                :correct="answer.correct"
                @click.prevent="handleClick(answer.correct)"
            >{{ answer.answer }}</button>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        handleClick(isCorrect) {
            this.$emit('questionChanged', isCorrect);
        },

        shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }

            return array;
        }
    },

    props: ['questionText', 'answers', 'isFinished']
}
</script>

<style lang="scss">
.c-flashcard {
	max-width: 400px;
	width: 100%;
	margin: 40px;
	border-radius: 8px;
	border: 0;
	background-color: $color-primary;
	box-shadow: $shadow-primary;
	overflow: hidden;

	@include breakpoint(500px) {
		margin: 0;
	}


	&__text-wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		max-height: 220px;
		height: 220px;
		padding: 40px;

		@include breakpoint(500px) {
			padding: 20px;
			height: 160px;
		}

		&--text-center {
			text-align: center;
		}
	}

	&__text {
		font-size: 20px;

		@include breakpoint(500px) {
			font-size: 18px;
		}
	}

	&__buttons-wrapper {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		height: 150px;
		background-color: $color-tertiary;

		@include breakpoint(500px) {
			height: 120px;
		}
	}

	&__btn {
		height: 100%;
		width: 50%;
		color: $color-primary;
		font-size: 16px;
		font-weight: 600;
		background-color: $color-secondary;
		text-transform: uppercase;
		line-height: 17px;
		padding: 10px;
		border: 0;
		outline: 0;

		@include breakpoint(500px) {
			font-size: 14px;
		}

		&:nth-child(odd) {
			background-color: $color-tertiary;
		}

		&:hover, &:focus {
			cursor: pointer;
		}
	}
}
</style>
