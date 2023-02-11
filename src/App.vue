<template>
    <!--button v-on:click="add_last()">追加</button-->
    <div class="hello">
        <draggable v-model="data" group="people" item-key="id" tag="ul">
            <template #item="{element, index}">
              <div style="display:flex;margin:10px">
                <li @click="liclick(index)">{{element.id}}
                  {{element.active}}
                </li>
                <li>
                  <span class="del" v-on:click="del(index)">[削除]</span>
                </li>
                <li>
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
                    active: 1,
                    content: "テスト1",
                },
                {
                    id: 2,
                    active: 0,
                    content: "テスト2",
                },
                {
                    id: 3,
                    active: 0,
                    content: "テスト3",
                },
            ],
        };
    },
    methods: {
      active(index) {
        for (var i=0;i<this.data.length;i++)
        {
          this.data[i]["active"] = 0; 
        }

        this.data[index]["active"] = 1; 

        for (i=0;i<this.data.length;i++)
        {
          console.log(this.data[i]["active"]); 
        }
      },
      add_last() {
        this.data.push( 
          {
              id: 3,
              active: 0,
              content: "テスト3",
          }
        );
      },
      add(index) {
        this.data.splice(index+1, 0, 
          {
              id: 3,
              active: 0,
              content: "テスト3",
          }
        );
        this.active(index+1)
      },
      del(index) {
        this.data.splice(index, 1);
      },
      liclick(index) {
        console.log("test")
        this.active(index)
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
