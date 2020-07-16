# JS Question Record

- JS code have syntax errors, entire JS method will don't work.

- Codes of after $.ajax({}) will execute.

- Early end a JS function, don't foget add return setence.

- After removing DOM element by $("").remove(), $(this) of the dom will be invalid.

- Binding a array DOMs, such as multiple `<select>` dom. Right solution: when HTML load, js need add bind event on existing `<select>`, and after dynamic add `<select>`, also need add bind event on these `<select>`.  
