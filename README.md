# react-gojs-tutorial

This document provides information on how the GoJs library uses it in ReactJs.

## Create react application
<code>yarn create react-app react-gojs-demo</code>

## Install GoJs library in project
<code>yarn add react-gojs</code>

## Insert code in index.js file
<code>import * as go from "gojs";</code>

<code>
const gojsKey = process.env.REACT_APP_GOJS_KEY;

if (gojsKey) {
    go.licenseKey = gojsKey;
}
</code>
