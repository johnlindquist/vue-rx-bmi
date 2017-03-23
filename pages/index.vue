<template>
  <div>
    <div>
      <div>Weight {{weight}} kg</div>
      <input type="range" min="40" max="140" v-stream:input="weight$">
    </div>
    <div>
      <div>Height {{height}} cm</div>
      <input type="range" min="140" max="210" v-stream:input="height$">
    </div>
    <h2>BMI is {{bmi}}</h2>
  </div>
</template>
<script>
import {Observable, Subject} from 'rxjs'

export default {
   subscriptions () {
    this.weight$ = new Subject()
    this.height$ = new Subject()

    const w$ = this.weight$
        .map(o => o.event.target.value)
        .startWith(70)

    const h$ = this.height$
        .map(o => o.event.target.value)
        .startWith(170)

    const bmi$ = Observable.combineLatest(w$, h$)
        .map(([weight, height]) => Math.round(weight / (height * 0.01)**2))

    return {
      weight: w$,
      height: h$,
      bmi: bmi$
    }
  }
}
</script>