# react-gojs-tutorial

This document provides information on how the GoJs library uses it in ReactJs.

## Create react application
```yarn create react-app react-gojs-demo```

## Install GoJs library in project
```yarn add react-gojs```

## Insert code in index.js file
```import * as go from "gojs";```

```
const gojsKey = process.env.REACT_APP_GOJS_KEY;

if (gojsKey) {
    go.licenseKey = gojsKey;
}
```

## Create a folder
```
-src
--DiagramView
---DiagramView.css
---index.js
```
### Insert code in Diagram/Diagram.css
```
.myDiagram {
    width: 70%;
    height: 100%;
    flex: 1 1 auto;
    margin: auto;
}
canvas {
    outline: none;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0); /* mobile webkit */
}
```

### Insert code in Diagram/index.js
```
import React from "react";

import * as go from "gojs";
import { ToolManager, Diagram } from "gojs";
import { GojsDiagram } from "react-gojs";

import "./DiagramView.css";

class DiagramView extends React.Component {
  constructor(props) {
    super(props);
    this.createDiagram = this.createDiagram.bind(this);

    this.state = {
      selectedNodeKeys: [],
      model: {
        nodeDataArray: [
          { key: "Alpha", label: "Alpha", color: "lightblue" },
          { key: "Beta", label: "Beta", color: "orange" },
          { key: "Gamma", label: "Gamma", color: "lightgreen" },
          { key: "Delta", label: "Delta", color: "pink" },
          { key: "Omega", label: "Omega", color: "grey" }
        ],
        linkDataArray: [
          { from: "Alpha", to: "Beta" },
          { from: "Alpha", to: "Gamma" },
          { from: "Beta", to: "Delta" },
          { from: "Gamma", to: "Omega" }
        ]
      }
    };
  }

  render() {
    return [
      <GojsDiagram
        key="gojsDiagram"
        diagramId="myDiagramDiv"
        model={this.state.model}
        createDiagram={this.createDiagram}
        className="myDiagram"
      />
    ];
  }

  createDiagram(diagramId) {
    const $ = go.GraphObject.make;

    const myDiagram = $(go.Diagram, diagramId, {
      initialContentAlignment: go.Spot.LeftCenter,
      layout: $(go.TreeLayout, {
        angle: 0,
        arrangement: go.TreeLayout.ArrangementVertical,
        treeStyle: go.TreeLayout.StyleLayered
      }),
      isReadOnly: false,
      allowHorizontalScroll: true,
      allowVerticalScroll: true,
      allowZoom: false,
      allowSelect: true,
      autoScale: Diagram.Uniform,
      contentAlignment: go.Spot.LeftCenter
    });

    myDiagram.toolManager.panningTool.isEnabled = false;
    myDiagram.toolManager.mouseWheelBehavior = ToolManager.WheelScroll;

    myDiagram.nodeTemplate = $(
      go.Node,
      "Auto",
      $(
        go.Shape,
        "RoundedRectangle",
        { strokeWidth: 0 },
        new go.Binding("fill", "color")
      ),
      $(
        go.TextBlock,
        { margin: 8, editable: true },
        new go.Binding("text", "label")
      )
    );

    return myDiagram;
  }
}

export default DiagramView;
```
