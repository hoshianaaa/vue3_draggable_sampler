<template>
    <!--button v-on:click="add_last()">追加</button-->
    <div class="hello">
        <draggable v-model="data" group="people" item-key="id" tag="ul">
            <template #item="{element, index}">
              <div style="display:flex;margin:10px">
                <li @click="liclick(index)">{{element.app}} index: {{element.id}}
                  select: {{element.select}}
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
import ROSLIB from "roslib" // 追加

const ros =  new ROSLIB.Ros({
    url: 'ws://localhost:9090'
});

export default {
    components: {
        draggable,
    },
    data() {
        return {
            data: [
                {
                    app: "edge_detection",
                    id: 1,
                    select: 1,
                    content: "テスト1",
                },
                {
                    app: "edge_detection",
                    id: 2,
                    select: 0,
                    content: "テスト2",
                },
                {
                    app: "scratch_detection",
                    id: 3,
                    select: 0,
                    content: "テスト3",
                },
            ],
        };
    },
    mounted() {
      this.init();
    },
    methods: {
      init: function () {
        ros.on("connection", function() {
          console.log("connected to websocket server.");
        });
        ros.on("error", function(error) {
          console.log("error connecting to websocket server: ", error);
        });
        ros.on("close", function() {
          console.log("connection to websocket server closed.");
        });
      },
      select(index) {
        for (var i=0;i<this.data.length;i++)
        {
          this.data[i]["select"] = 0; 
        }

        this.data[index]["select"] = 1; 

        for (i=0;i<this.data.length;i++)
        {
          console.log(this.data[i]["select"]); 
        }
      },
      add_last() {
        this.data.push( 
          {
              app: "edge_detection",
              id: 3,
              select: 0,
              content: "テスト3",
          }
        );
      },
      add(index) {
        this.data.splice(index+1, 0, 
          {
              app: "edge_detection",
              id: 3,
              select: 0,
              content: "テスト3",
          }
        );
        this.select(index+1)
      },
      del(index) {
        this.data.splice(index, 1);
      },
      liclick(index) {
        console.log("test")
        this.select(index)
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
