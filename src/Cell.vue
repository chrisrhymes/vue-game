<template>
    <td>
        <button class="button is-fullwidth is-medium" @click="shout()" :class="[{'is-primary': value === 'X', 'is-dark' : value === 'O'}]">
            {{ value }}
        </button>
    </td>
</template>

<script>
    export default {
        name: 'cell',
        data()
        {
            return {
                value: '-'
            }
        },
        methods:
        {
            shout()
            {
                if(this.value === '-')
                {
                    this.$parent.updatePlayer();
                    this.value = this.$parent.currentPlayer;
                    this.$parent.checkResult();
                }
            }
        }
    }
</script>