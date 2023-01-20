<template>
	<div class="wr">
		<div class="p-3 rounded-3" style="max-width: 600px; margin: 60px auto; background: #233656">
			<div class="w-full rounded-3 m-1 p-3 text-md-end lead font-weight-bold text-black screen-bg"
				style="font-weight: 700;font-size: 32px;">
				{{ calcValue|| 0}}
			</div>
			<div class="row">
				<div class="col-3" v-for="el in calcElements" :key="el">
					<div
              class="lead text-center m-1 py-3 el-bg rounded-2"
              :class="{
						    'bg-operators': ['C', '*', '/', '-', '+', '*', '%', '='].includes(el),
						    'sound': ['Sound'].includes(el),
					    	'soundActive': (['Sound'].includes(el) && soundActive),
				       }"
               id="btn"
              @click.prevent="
                playSound( require('../assets/sounds/clickSound.mp3') )
                action(el);

              "
          >
						{{ el }}
					</div>

				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Calculator',
	data() {
		return {
			calcValue: '',
			calcElements: [
				'C', '*', '/', '-',
				'7', '8', '9', '+',
				'4', '5', '6', '%',
				'1', '2', '3', '=',
				'0', '.', 'Sound'
			],
			calcPrevValue: '',
			operator: null,
			soundActive: false,
		}
	},
	methods: {
		action(el) {
			if (!isNaN(el) || el === '.') {
				this.calcValue += el + ''
			}
			if (el === 'C') {
				this.calcValue = ''
			}
			if (el === '%') {
				this.calcValue = this.calcValue / 100 + ''
			}
			if (['*', '/', '-', '+'].includes(el)) {
				this.operator = el;
				this.calcPrevValue = this.calcValue + ''
				this.calcValue = ''
			}
			if (el === '=') {
				this.calcValue = eval(this.calcPrevValue + this.operator + this.calcValue)
				this.calcPrevValue = ''
				this.operator = null
			}
			if (el === 'Sound') {
				this.soundActive = !this.soundActive
			}

		},
    playSound (sound) {
      if(this.soundActive){
        if(sound) {
          let audio = new Audio(sound);
          audio.play();
        }
      }
    }
	}
}
</script>

<style scoped>
.screen-bg {
	background: #CDD6D5;
}

.el-bg {
	background: #415B76;
	user-select: none;
}

.el-bg:hover {
	cursor: pointer;
	background: #3c546d;
}

.bg-operators {
	cursor: pointer;
	background: #7B9BA6;
}

.bg-operators:hover {
	background: #7797a0;
}

#btn {
	color: white;
	font-weight: 700;
	font-size: 32px;
}

.sound {
	background: #439643;
}
.sound:hover {
  background: #439643;
}

.soundActive {
  background: #5bce5b;

	-moz-box-shadow: 0 0 10px #fff;
	-webkit-box-shadow: 0 0 10px #fff;
	box-shadow: 0 0 10px #fff;
}

@media (max-width: 468px) {
	.el-bg {
		width: 2.1em;
	}
}

@media (max-width: 380px) {
	.el-bg {
		width: 1.5em;
	}
}
@media (max-width: 550px) {
  .sound {
    width: 120px;
  }

}
</style>
