<template>
    <button v-on:click="add_last()">追加</button>
    <div class="hello">
        <draggable v-model="data" group="people" item-key="id" tag="ul">
            <template #item="{element, index}">
              <div>
                <li>{{element.id}}
                  <span class="del" v-on:click="del(index)">[削除]</span>
                  <span class="add" v-on:click="add(index)">[追加]</span>
                </li>
              </div>
            </template>
        </draggable>
    </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
    components: {
        draggable,
    },
    data() {
        return {
            data: [
                {
                    id: 1,
                    content: "テスト1",
                },
                {
                    id: 2,
                    content: "テスト2",
                },
                {
                    id: 3,
                    content: "テスト3",
                },
            ],
        };
    },
    methods: {

      add_last() {
        console.log("test")
        this.data.push( 
          {
              id: 3,
              content: "テスト3",
          }
        );
      },
      add(index) {
        console.log("test")
        this.data.splice(index+1, 0, 
          {
              id: 3,
              content: "テスト3",
          }
        );
      },
      del(index) {
        console.log("test")
        this.data.splice(index, 1);
      },

    },
    
};
</script>

<style scoped>
ul {
  list-style-type: none;
}
li {
  cursor: move;
  padding: 10px;
  border: solid #ddd 1px;
}
</style>
