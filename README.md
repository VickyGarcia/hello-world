<!DOCTYPE html>
<html>
<head>
    <title>Nueva Solicitud</title>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.11.4/jquery-ui.min.js"></script>
  <script src="https://formbuilder.online/assets/js/form-builder.min.js"></script>
<div class="setDataWrap">
  <button id="getXML" type="button">Get XML Data</button>
  <button id="getJSON" type="button">Get JSON Data</button>
</div>
<div id="build-wrap"></div>
  <!--<div id="fb-editor"></div>-->
  
</body>
</html>

<script>
jQuery($ => {
    var options = {
      //idioma
      i18n: {
        locale: 'es-ES'
      },
      //boton gurdar del formulario
      onSave: function(evt, formData) {
      toggleEdit(false);
      alert("Guardado correctamente"+formBuilder.actions.getData('json'));  
      $('.render-wrap').formRender({formData});
    }
    };
    
  //Construye el formulario
  $fbTemplate = $(document.getElementById('build-wrap'));
  var formBuilder = $fbTemplate.formBuilder(options);

  document.getElementById('getXML').addEventListener('click', function() {
    alert(formBuilder.actions.getData('xml'));
  });
  document.getElementById('getJSON').addEventListener('click', function() {
    alert(formBuilder.actions.getData('json'));
  });
});

function toggleEdit(editing) {
  document.body.classList.toggle('form-rendered', !editing);
}

document.getElementById('edit-form').onclick = function() {
  toggleEdit(true);
};
</script>

https://github.com/bezael/sistema-login-php-mysql-jquery
https://github.com/WilliamRiveraRamos/PHP-login




<!DOCTYPE html>
<html>
<head>
    <title>Nueva Solicitud</title>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.11.4/jquery-ui.min.js"></script>
  <script src="https://formbuilder.online/assets/js/form-builder.min.js"></script>
  <script src="https://formbuilder.online/assets/js/form-render.min.js"></script>
  <link rel="stylesheet" href="css/estilo.css" type="text/css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.6/css/bootstrap.min.css" type="text/css">
  
  <!-- <script src="js/funciones.js" language="javascript" type="text/javascript"></script>-->

<div class="setDataWrap">
  <button id="getXML" type="button" class="btn-info btn">Get XML Data</button>
  <button id="getJSON" type="button" class="btn-info btn">Get JSON Data</button>
</div>

<div id="build-wrap"></div>
<div class="render-wrap"></div>
<div class="saveDataWrap">
  <button id="saveData" type="button" class="btn-success btn">Guardar formulario</button>
</div>
<button id="edit-form" class="btn-warning btn">Editar</button>


<script type="text/javascript">


  jQuery($ => {
    var options = {
      //idioma
      i18n: {
        locale: 'es-ES'
      },
      //boton gurdar del formulario
      onSave: function(evt, formData) {
        toggleEdit(false);
        //alert("Guardado correctamente"+formBuilder.actions.getData('json'));
        $('.render-wrap').formRender({formData});
      }
    };
    
  //Construye el formulario
  $fbTemplate = $(document.getElementById('build-wrap'));
  var formBuilder = $fbTemplate.formBuilder(options);

  document.getElementById('getXML').addEventListener('click', function() {
    alert(formBuilder.actions.getData('xml'));
  });
  document.getElementById('getJSON').addEventListener('click', function() {
    alert(formBuilder.actions.getData('json'));
  });

  document.getElementById("saveData").addEventListener("click", () => formBuilder.actions.save());
  });

/**
 * Toggles the edit mode for the demo
 * @return {Boolean} editMode
 */
function toggleEdit(editing) {
  document.body.classList.toggle('form-rendered', !editing);
}

document.getElementById('edit-form').onclick = function() {
  toggleEdit(true);
};

</script>
  
</body>
</html>




<html>
<head>
    <title>Nueva Solicitud</title>
</head>
<body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.11.4/jquery-ui.min.js"></script>
  <script src="https://formbuilder.online/assets/js/form-builder.min.js"></script>
  <script src="https://formbuilder.online/assets/js/form-render.min.js"></script>
  <link rel="stylesheet" href="css/estilo.css" type="text/css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.6/css/bootstrap.min.css" type="text/css">
  
  <!-- <script src="js/funciones.js" language="javascript" type="text/javascript"></script>-->

<form id="fb-render"></form>
<button type="button" id="get-user-data" class="btn-success btn">enviar</button>

<script type="text/javascript">

  /*
This has been updated to use the new userData method available in formRender
*/
const getUserDataBtn = document.getElementById("get-user-data");
const fbRender = document.getElementById("fb-render");
const originalFormData = [
  {
    type: "text",
    label: "Text Field",
    className: "form-control",
    name: "text-1478701075825"
  },
  {
    type: "checkbox-group",
    label: "Checkbox Group",
    className: "checkbox-group",
    name: "checkbox-group-1478704652409",
    values: [
      {
        label: "Option 1",
        value: "option-1",
        selected: true
      },
      {
        label: "Option 2",
        value: "option-2"
      },
      {
        label: "Option 3",
        value: "option-3",
        selected: true
      }
    ]
  },
  {
    type: "select",
    label: "Select",
    className: "form-control",
    name: "select-1478701076382",
    values: [
      {
        label: "Option 1",
        value: "option-1",
        selected: true
      },
      {
        label: "Option 2",
        value: "option-2"
      },
      {
        label: "Option 3",
        value: "option-3"
      }
    ]
  },
  {
    type: "textarea",
    label: "Text Area",
    className: "form-control",
    name: "textarea-1478701077511"
  }
];
jQuery(function($) {
  const formData = JSON.stringify(originalFormData);

  $(fbRender).formRender({ formData });
  getUserDataBtn.addEventListener(
    "click",
    () => {
      window.alert(window.JSON.stringify($(fbRender).formRender("userData")));
    },
    false
  );
});
</script>
  
</body>
</html>



body {
  font-family: sans-serif;
  padding: 10;
  margin: 10px 0;
  background: #f2f2f2 url('http://formbuilder.readthedocs.io/en/latest/img/noise.png');
}

.form-rendered #build-wrap {
  display: none;
}

.render-wrap {
  display: none;
}

.form-rendered .render-wrap {
  display: block;
}

#edit-form {
  display: none;
  float: right;
}

.form-rendered #edit-form {
  display: block;
}

#guardarfor {
  display: none;
  float: right;
}

.form-rendered #guardarfor {
  display: block;
}

#saveData {
  display: none;
  float: right;
}

.form-rendered #saveData {
  display: block;
}

 .setDataWrap {
  text-align: center;
  margin-top: 30px;
  margin-bottom: 30px;
}

textarea.form-control {
  height: 120px;
}


ery($ => {
    var options = {
      //idioma
      i18n: {
        locale: 'es-ES'
      },
      //boton gurdar del formulario
      onSave: function(evt, formData) {
      toggleEdit(false); 
      $('.render-wrap').formRender({formData});
      alert("Guardado correctamente"+formBuilder.actions.getData('json')); 
      }
    };
    
  //Construye el formulario
  $fbTemplate = $(document.getElementById('build-wrap'));
  var formBuilder = $fbTemplate.formBuilder(options);

  document.getElementById('getXML').addEventListener('click', function() {
    alert(formBuilder.actions.getData('xml'));
  });
  document.getElementById('getJSON').addEventListener('click', function() {
    alert(formBuilder.actions.getData('json'));
  });
});

/**
 * Toggles the edit mode for the demo
 * @return {Boolean} editMode
 */
function toggleEdit(editing) {
  document.body.classList.toggle('form-rendered', !editing);
}

document.getElementById('edit-form').onclick = function() {
  toggleEdit(true);
};
