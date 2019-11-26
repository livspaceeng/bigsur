<template>
  <div class="content" ref="container">
    <div class="canvas" ref="canvas"></div>
    <div class="properties-panel-parent" id="js-properties-panel"></div>
  </div>
</template>

<script>
// import BpmnModeler from 'bpmn-js/lib/Modeler';
import BpmnModeler from '../modeler';
// import { debounce } from 'min-dash';
import propertiesPanelModule from 'bpmn-js-properties-panel';
import propertiesProviderModule from 'bpmn-js-properties-panel/lib/provider/camunda';
// import camundaModdleDescriptor from 'camunda-bpmn-moddle/resources/camunda.json';

export default {
  name: 'HelloWorld',
  data(){
      return {
        id: "",
        bpmnModeler: null,
        container: null,
        canvas: null,
        xmlStr : `
<?xml version="1.0" encoding="UTF-8"?>
<bpmn2:definitions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:bpmn2="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" xsi:schemaLocation="http://www.omg.org/spec/BPMN/20100524/MODEL BPMN20.xsd" id="sample-diagram" targetNamespace="http://bpmn.io/schema/bpmn">
  <bpmn2:process id="Process_1" isExecutable="false">
    <bpmn2:startEvent id="StartEvent_1"/>
  </bpmn2:process>
  <bpmndi:BPMNDiagram id="BPMNDiagram_1">
    <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="Process_1">
      <bpmndi:BPMNShape id="_BPMNShape_StartEvent_2" bpmnElement="StartEvent_1">
        <dc:Bounds height="36.0" width="36.0" x="412.0" y="240.0"/>
      </bpmndi:BPMNShape>
    </bpmndi:BPMNPlane>
  </bpmndi:BPMNDiagram>
</bpmn2:definitions>`,
                 // Click on the data obtained by the node
        nodeCode: "",
        nodeName: ""
      }
  },
  
  props: {
    msg: String
  },
  methods: {
    createNewDiagram: function() {
        this.openDiagram(this.xmlStr);
    },

    openDiagram: function(xml) {
        this.bpmnModeler.importXML(xml, function(err) {
            if (err) {
                console.error(err)
            } else {
              console.log("Imported xml");
            }
        });
    }
  },
  mounted(){
    this.id = this.bpmnId;
    this.container = this.$refs.container;
    var canvas = this.$refs.canvas;
    this.bpmnModeler = new BpmnModeler({
        container: canvas,
        propertiesPanel: {
            parent: '#js-properties-panel'
        },
        additionalModules: [
            propertiesPanelModule,
            propertiesProviderModule
        ]
        /*,
        moddleExtensions: {
            camunda: camundaModdleDescriptor
        }*/
    });
    this.createNewDiagram();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
// @import "bpmn-js-properties-panel/styles/properties";

* {
  box-sizing: border-box;
}

body,
html {

  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;

  font-size: 12px;

  height: 100%;
  max-height: 100%;
  padding: 0;
  margin: 0;
}

a:link {
  text-decoration: none;
}

.content {
  position: relative;
  width: 100%;
  height: 100%;

  > .message {
    width: 100%;
    height: 100%;
    text-align: center;
    display: table;

    font-size: 16px;
    color: #111;

    .note {
      vertical-align: middle;
      text-align: center;
      display: table-cell;
    }

    &.error {
      .details {
        max-width: 500px;
        font-size: 12px;
        margin: 20px auto;
        text-align: left;
        color: #BD2828;
      }

      pre {
        border: solid 1px #BD2828;
        background: #fefafa;
        padding: 10px;
        color: #BD2828;
      }
    }
  }
  &:not(.with-error) .error,
  &.with-error .intro,
  &.with-diagram .intro {
    display: none;
  }

  .canvas {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  .canvas,
  .properties-panel-parent {
    display: none;
  }

  &.with-diagram {
    .canvas,
    .properties-panel-parent  {
      display: block;
    }
  }
}


.buttons {
  position: fixed;
  bottom: 20px;
  left: 20px;

  padding: 0;
  margin: 0;
  list-style: none;

  > li {
    display: inline-block;
    margin-right: 10px;

    > a {
      background: #DDD;
      border: solid 1px #666;
      display: inline-block;
      padding: 5px;
    }
  }

  a {
    opacity: 0.3;
  }

  a.active {
    opacity: 1.0;
  }
}

.properties-panel-parent {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  width: 260px;
  z-index: 10;
  border-left: 1px solid #ccc;
  overflow: auto;
  &:empty {
    display: none;
  }
  > .djs-properties-panel {
    padding-bottom: 70px;
    min-height:100%;
  }
}
</style>
