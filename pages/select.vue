<template>
<div class="container" style="display:flex;justify-content:center;align-items:center;flex-direction:column">
    <h1>Select your intrests</h1>
    <div style="display:flex;justify-content:center;align-items:center;flex-wrap: wrap;">
        <button class="intrest" @click="Add(7119)" id='7119'>Health</button>
        <button class="intrest" @click="Add(234)" id='234'>Art</button>
        <button class="intrest" @click="Add(509715)" id='509715'>Robotics</button>
        <button class="intrest" @click="Add(1043)" id='1043'>Music</button>
        <button class="intrest" @click="Add(1038)" id='1038'>Photography</button>
        <button class="intrest" @click="Add(3065)" id='3065'>Culture</button>
        <button class="intrest" @click="Add(2464)" id='2464'>Food</button>
        <button class="intrest" @click="Add(225085)" id='225085'>Fitness</button>
        <button class="intrest" @click="Add(9647)" id='9647'>Startups</button>
        <button class="intrest" @click="Add(382874)" id='382874'>Creativity</button>
        <button class="intrest" @click="Add(1152)" id='1152'>Politics</button>
        <button class="intrest" @click="Add(2872452)" id='2872452'>Children</button>
        <button class="intrest" @click="Add(1019)" id='1019'>Business</button>
        <button class="intrest" @click="Add(814)" id='814'>Science</button>
    </div>
    <button @click="Done" class="submit">Done</button>
</div>
</template>

<script>
export default {
    data() {
        return {
            Selected: [],
            search: '',
            items: [],
            exact: true,
            AlreadyAdded: [],
            ButtonText: 'Add'
        }
    },
    methods: {
        Done() {
            this.$cookies.set('interests', this.Selected)
            this.$router.push('/')
        },
        Add(id) {
            console.log(id)
            var AlreadyAdded = false;
            var ref = document.getElementById(id)

            if (this.Selected.includes(id) == true) {
                AlreadyAdded = true;
            }

            if (AlreadyAdded == true) {
                ref.classList.remove('selected')
                this.Selected.indexOf(id) > -1 ? this.Selected.splice(this.Selected.indexOf(id), 1) : false
            } else {
                if (this.Selected.length >= 5) {
                    Swal.fire({
                        icon: 'error',
                        title: "You can't add more than 5 interests"
                    })
                } else {
                    ref.classList.add('selected')
                    this.Selected.push(id)
                }
            }

        }
    }
}
</script>

<style scoped>
.container {
    background:#F1F7FD;
    color:#FD379E;
    text-shadow: 2px 2px #000;
    width:100vw;
    height:100vh;
}

.intrest {
    background-color: transparent;
    padding-left: 15px;
    padding-right: 15px;
    padding-top: 5px;
    padding-bottom: 5px;
    color: #000;
    border-radius: 4px;
    border: 1.5px solid #222222;
    margin-left: 10px;
    transition: 0.2s linear;
    margin-top: 15px;
}

.selected {
    -webkit-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
    -moz-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
    box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
}

.submit {
    margin-top: 2rem;
    background: #9BFEB5;
    width: 10rem;
    height: 2rem;
    border-radius:4px;
    border:1px solid #222222;
    transition:0.2s ease;
}
.submit:hover {
        -webkit-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
    -moz-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
    box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 0.67);
}

*:focus {
outline: 0;
}
</style>
