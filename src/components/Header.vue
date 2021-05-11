<template>
  <div>
  <!---bootstrapの使用:tabs------>
  <b-nav tabs>
  <!-- question point--->
  <!-- nav barの使用だったため...disabledにすることでボタンを押せなくする--->
  <b-nav-item disabled><b>Facny Quiz App</b></b-nav-item>          
  <b-nav-item disabled>Counter:{{ numCorrect }}/{{ numTotal }}</b-nav-item>
  </b-nav>
    </div>
</template>
<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>