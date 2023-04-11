// ==UserScript==
// @name         AutoRefresh
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       khoxphan
// @match        https://aftlite-na.amazon.com/labor_tracking/labor_summary*
// @match        https://aftlite-na.amazon.com/labor_tracking/view_daily_detail*
// @match        https://aftlite-na.amazon.com/labor_tracking/uph_drilldown*
// @match        https://aftlite-portal.amazon.com/labor_tracking/view_daily_detail*
// @match        https://aftlite-eu.amazon.com/labor_tracking/labor_summary*
// @match        https://aftlite-eu.amazon.com/labor_tracking/view_daily_detail*
// @match        https://aftlite-eu.amazon.com/labor_tracking/uph_drilldown*
// @match        https://aftlite-portal-eu.amazon.com/labor_tracking/view_daily_detail*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

// create checkbox element and label
var checkbox = document.createElement('input');
checkbox.type = "checkbox";
checkbox.style.float = "right"; // align checkbox to right
var label = document.createElement('label');
label.appendChild(document.createTextNode('Auto-refresh'));

// create container div for checkbox and label
var container = document.createElement('div');
container.style.position = "absolute";
container.style.top = "10px"; // adjust top position as needed
container.style.right = "10px"; // adjust right position as needed
container.appendChild(checkbox);
container.appendChild(label);

// append container to the document
document.body.appendChild(container);

// function to toggle auto-refresh based on checkbox state
function toggleAutoRefresh() {
    if (checkbox.checked) {
        // enable auto-refresh
        window.autoRefreshId = setInterval(function() {
            location.reload();
        }, 30000);
        localStorage.setItem('autoRefreshEnabled', 'true');
    } else {
        // disable auto-refresh
        clearInterval(window.autoRefreshId);
        localStorage.setItem('autoRefreshEnabled', 'false');
    }
}

// add event listener to checkbox
checkbox.addEventListener('change', toggleAutoRefresh);

// load initial state from local storage
var autoRefreshEnabled = localStorage.getItem('autoRefreshEnabled');
if (autoRefreshEnabled === 'true') {
    checkbox.checked = true;
    toggleAutoRefresh();
}
