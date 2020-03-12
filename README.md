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
