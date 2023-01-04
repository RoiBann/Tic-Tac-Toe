<script>
export default {
  name: 'MorpionView',
  data() {
    return {
      status: "Le joueur X doit commencer.",
      jeuActif: true,
      joueurActif: "X",
      etatJeu: ["", "", "", "", "", "", "", "", ""],

      conditionsVictoire: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
       
    }
  },
  methods: {
    // Fonction qui gère le clique sur les cases
    gestionClicCase: function(indexCase){
      if (this.etatJeu[indexCase] != "" || !this.jeuActif){
        return
      }
      this.etatJeu[indexCase] = this.joueurActif

      this.verifgagne();
    },
    // Fonction qui vérifie s'il y a un vainqueur ou égalité
    verifgagne: function (){
      
      let tourGagnant = false

      // Vérifie si une des conditions de victoire est valide
      for (let conditionVictoire of this.conditionsVictoire){
        let val1 = this.etatJeu[conditionVictoire[0]]
        let val2 = this.etatJeu[conditionVictoire[1]]
        let val3 = this.etatJeu[conditionVictoire[2]]

        if (val1 == "" || val2 == "" || val3 == ""){
          continue
        }
        if (val1 == val2 && val2 == val3){
          tourGagnant = true
          break
        }
      }
      if (tourGagnant){
        this.status = `Le joueur ${this.joueurActif} a gagné`
        this.jeuActif = false
        return
      }
      // Vérifie s'il y a égalité
      if (!this.etatJeu.includes("")){
        this.status = `Egalité`
        this.jeuActif = false
        return
      }
      this.joueurActif = this.joueurActif == "X" ? "O" : "X"
      this.status = `Joueur ${this.joueurActif} doit jouer`
    },
    // Fonction permettant de réinitialiser le jeu et les données
    recommencer: function (){
      this.joueurActif = "X"
      this.jeuActif = true
      this.etatJeu = ["", "", "", "", "", "", "", "", ""]
      this.status = `Joueur ${this.joueurActif} doit jouer`
    }
  },
}
</script>
<template>

  <main>
    <h1 class="morpion">Morpion</h1>
    <Transition>
      <section id="jeu" v-if="jeuActif">
        <div v-for="(item, index) in etatJeu" :key="index" data-index="0" class="case" @click="gestionClicCase(index)">
          <span :class="item">{{ item }}</span>
        </div>     
      </section>
      <div class="image" v-else>
        <img src="https://www.vcmb-venissieux.com/media/uploaded/sites/6462/actualite/5b154b7aa6979_coupevictoire.png"/>
      </div>
    </Transition>

    <button id="recommencer" @click="recommencer">Recommencer</button>
      <div>
        <h2 class="status">{{ status }}</h2>
      </div>
  </main>
</template>

<style scoped>


#jeu {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    width: 300px;
    margin: auto;
    background-color: #FBF1C6;
}

.case {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 6em;
    border: 2px solid #C7B461;
    height: 100px;
}

.case .X {
  color: blue;
}

.case .O {
  color: brown;
}

#recommencer {
    margin: 10px auto;
    background-color: #FBF1C6;
    border-color: #C7B461;
}

.morpion {
  color: #C7B461;
  -webkit-text-stroke: #7A6F3C 1px;
  letter-spacing: 0.4rem;
}

.v-enter-active {
  transition: opacity 0.5s ease;
}

.v-enter-from{
  opacity: 0;
}
.v-enter-to {
  opacity: 1;
}

/* .v-leave-from {
  opacity: 1;
}

.v-leave-to {
  opacity: 0.5;
} */

</style>
