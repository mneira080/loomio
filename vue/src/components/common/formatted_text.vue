<script lang="coffee">
export default
  props:
    model:
      type: Object
      required: true

    column:
      type: String
      required: true

  computed:
    isMd: -> @format == 'md'
    isHtml: -> @format == 'html'
    format: -> @model[@column+"Format"]
    text: -> @model[@column]
    hasTranslation: -> @model.translation
    cookedText: ->
      return @model[@column] unless @model.mentionedUsernames
      cooked = @model[@column]
      @model.mentionedUsernames.forEach (username) ->
        cooked = cooked.replace(///@#{username}///g, "[[@#{username}]]")
      cooked
</script>

<template lang="pug">
div.lmo-markdown-wrapper
  div(v-if="!hasTranslation && isMd" v-marked='cookedText')
  div(v-if="!hasTranslation && isHtml" v-html='text')
  translation(v-if="hasTranslation" :model='model' :field='column')
</template>