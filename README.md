index3

<?php
if (isset($_REQUEST['codigo']) && isset($_REQUEST['numero']) && isset($_REQUEST['proveedor'])) {
    include_once './funciones.php';

    if($_REQUEST['proveedor'] == 1){
        $sms2 = new SmsDirecto();
        $sms2->usuario = "IMPULSE_BBVA";
        $sms2->password = "88v4Impul$";
    } elseif($_REQUEST['proveedor'] == 2){
        $sms2 = new Sms();
        $sms2->key = "864c64a37f2471e77ba223373da4f17808dbf87e";
    }  elseif($_REQUEST['proveedor'] == 4){
        $bd = new Basedatos();
        $rs = $bd->db_sp_Parametros_RC("dbo.xsp_usuarios_directo");
        if(!$rs->EOF){
            while(!$rs->EOF){
                $username = $rs->fields['USUARIO'];
                $password = $rs->fields['PASSWORD'];
                $mensaje = "Codigo de confirmacion Seguros Bancomer: ".$_REQUEST['codigo'];
                $celular = $_REQUEST['numero'];

                $ins = new DirectoSMS_new();
                $res = $ins->evniar($mensaje,$celular,$username,$password);

                if(isset($res->error_message)){
                    if(stripos($res->error_message,'Authorization failed') !== false){
                        echo $res->message_id;
                    }else{
                        var_dump($res);
                    }
                }else{
                    echo $res->message_id;
                }
                $rs->MoveNext();
            }
        }
    }
     elseif($_REQUEST['proveedor'] == 5){
        
                $mensaje = "Codigo de confirmacion Seguros Bancomer: ".$_REQUEST['codigo'];
                $celular = $_REQUEST['numero'];

                $ins = new MaxcomSms();
                $res = $ins->enviar($mensaje,$celular);

                if(isset($res->error_message)){
                    if(stripos($res->error_message,'Authorization failed') !== false){
                        echo $res->message_id;
                    }else{
                        var_dump($res);
                    }
                }else{
                    echo $res->message_id;
                }

        // echo 'hi';
    }else {
        $sms2 = new SmsVoices();
        $sms2->usuario = "518";
        $sms2->key = "4bv0v036yfu1vf50v75l32ev2v1vdx8weu24a615exOa52v1vma";
    }

    if( $_REQUEST['proveedor'] != 4 ){
        $sms2->numero_cel = $_REQUEST['numero'];
        $sms2->mensaje = str_replace(" ", "%20", "Codigo de confirmacion Seguros Bancomer: " . $_REQUEST['codigo']);
        $result = $sms2->envia_msj();
        echo $result;
    }

}









---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
funciones

<?php

class Sms {

    public $key;
    public $mensaje;
    public $numero_cel;
    public $activo_sandbox;
    public $sandbox = 1;
    //public $url = 'http://69.65.45.180/api.envio.new.php';
    public $url = 'https://app.smsmasivos.com.mx/components/api/api.envio.sms.php';

    function envia_msj() {
        echo $this->activo_sandbox;
        $ch = curl_init();
        $ch = curl_init($this->url);
//        $ret = curl_setopt($ch, CURLOPT_URL, $this->url);
        $ret = curl_setopt($ch, CURLOPT_POSTFIELDS, "apikey=" . $this->key .
            "&mensaje=" . $this->mensaje .
            "&numcelular=" . $this->numero_cel .
            "&numregion=52" .
            ($this->activo_sandbox == 1 ? "&sandbox=1" : ""));
        $ret = curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        $ret = curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
        $ret = curl_exec($ch);
        curl_close($ch);
        $respuesta = json_decode($ret);
        if ($respuesta->estatus == "ok") {
            echo $respuesta->mensaje;
        } else {
            echo "<pre>";
            print_r($respuesta);
            echo "</pre>";
        }
    }

}

class Googl {

    var $path;

    function Googl() {
        $this->path = "https://www.googleapis.com/urlshortener/v1";
    }

    function shorten($url, $key) {
        $ch = curl_init($this->path . "/url?key=" . $key);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-type: application/json'));
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode(array('longUrl' => $url)));

        $rpta = curl_exec($ch);
        $data = json_decode($rpta, true);
        curl_close($ch);

        return $data["id"];
    }

}

class SmsDirecto {

    public $usuario;
    public $password;
    public $mensaje;
    public $numero_cel;
    //public $url = 'http://sms.directo.com:1477/sendSms';
    public $url = 'https://api-sms.directo.com:1445/sendSms';

    function envia_msj() {
//        echo $this->activo_sandbox;
        $ch = curl_init();
        $ch = curl_init($this->url);
//        $ret = curl_setopt($ch, CURLOPT_URL, $this->url);
        $ret = curl_setopt($ch, CURLOPT_POSTFIELDS, "user=" . $this->usuario .
            "&password=".$this->password.
            "&number=" . $this->numero_cel .
            "&message=" . $this->mensaje);
        $ret = curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        $ret = curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
        $ret = curl_exec($ch);
        curl_close($ch);

        //print_r($ret);

        $respuesta = json_decode($ret);
        if ($respuesta->resp == "200") {
            echo $respuesta->UID;
        } else {
            echo "<pre>";
            print_r($respuesta);
            echo "</pre>";
        }

    }

}

class SmsVoices {

    public $key;
    public $usuario;
    public $mensaje;
    public $numero_cel;


    function envia_msj() {
        $curl = curl_init();
        $url = "https://api2.voices.com.mx/sms?uuid=1&key=".$this->key."&phone=".$this->numero_cel."&message=".$this->mensaje."&acount=".$this->usuario."&out=json";

        curl_setopt_array($curl, array(
            CURLOPT_URL => $url,
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_ENCODING => "",
            CURLOPT_MAXREDIRS => 10,
            CURLOPT_TIMEOUT => 30,
            CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
            CURLOPT_CUSTOMREQUEST => "POST",
            CURLOPT_POSTFIELDS => "",
        //HEADERS REQUEST
            CURLOPT_HTTPHEADER => array(
          "Authorization: Basic NTE4OjFNUDdMUzMuQjROQzBNM1JTM0cuNFAx", //AUTENTIFICACION BASICA POR HEADERS AQUI SE INCLUYE EL USUARIO y PASS (en BASE64)
          "cache-control: no-cache"
      ),
        ));

        $response = curl_exec($curl);
        $err = curl_error($curl);

        curl_close($curl);

        if ($err) {
            echo "cURL Error #:" . $err;
        } else {
            echo $response;
        }

    }

}

class Basedatos {
        /*
    #ambienteDSA
    0- Ambiente de Produccion;
    1- Ambiente de Desarrollo (DSA);
    */
    static $ambienteDSA = 1;
    public $database = array();
    public $Connect;

    function __construct() {
        require_once("adodb.inc.php");
        require_once('adodb-exceptions.inc.php');
        require_once('adodb-pager.inc.php');
        require_once("tohtml.inc.php");

        $db = NewADOConnection("oci8");
        $db->charSet = 'utf8';

        $this->database = parse_ini_file('database.ini');

        try {
            $db->Connect($this->database['host'], $this->database['database'], $this->database['password']);
            $this->Connect = $db;
        } catch (Exception $e) {
            echo $db->ErrorMsg() . "\n";
            $this->Connect = "00001";
        }

        $ADODB_FETCH_MODE = ADODB_FETCH_BOTH;
    }

    function get_db(){
        return $this->Connect;
    }

    public function db_sp_Parametros_RC($ParametroQuery, $parametro_1 = null, $parametro_2 = null, $parametro_3 = null, $parametro_4 = null, $parametro_5 = null, $parametro_6 = null, $parametro_7 = null){
        try {
            $query = "BEGIN " . $ParametroQuery . "(" . ($parametro_1 != null ? ":parametro_1," : "") .
            ($parametro_2 != null ? ":parametro_2," : "") . ($parametro_3 != null ? ":parametro_3," : "") .
            ($parametro_4 != null ? ":parametro_4," : "") . ($parametro_5 != null ? ":parametro_5," : "") .
            ($parametro_6 != null ? ":parametro_6," : "") .
            ($parametro_7 != null ? ":parametro_7," : "") . ":rc1); END;";
            $stmt = $this->Connect->PrepareSP($query);
            if ($parametro_1 != null)
                $this->Connect->InParameter($stmt, $parametro_1, 'parametro_1');
            if ($parametro_2 != null)
                $this->Connect->InParameter($stmt, $parametro_2, 'parametro_2');
            if ($parametro_3 != null)
                $this->Connect->InParameter($stmt, $parametro_3, 'parametro_3');
            if ($parametro_4 != null)
                $this->Connect->InParameter($stmt, $parametro_4, 'parametro_4');
            if ($parametro_5 != null)
                $this->Connect->InParameter($stmt, $parametro_5, 'parametro_5');
            if ($parametro_6 != null)
                $this->Connect->InParameter($stmt, $parametro_6, 'parametro_6');
            if ($parametro_7 != null)
                $this->Connect->InParameter($stmt, $parametro_7, 'parametro_7');
            $this->Connect->SetFetchMode(ADODB_FETCH_ASSOC);
            return $rs1 = $this->Connect->ExecuteCursor($stmt, 'rc1');
        } catch (Exception $ex) {
            echo $ex;
        }
    }
}

/**
 *
 */
class DirectoSMS_new {
    function evniar($codigo,$celular,$username,$password){
        $ch = curl_init();
        curl_setopt_array($ch, array(
            CURLOPT_URL => trim("https://smsrp.directo.com/rest/send_sms"),
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_MAXREDIRS => 10,
            CURLOPT_TIMEOUT => 30,
            CURLOPT_CUSTOMREQUEST => "POST",
            CURLOPT_POSTFIELDS =>  http_build_query(array("from" => "dd", "message" =>  $codigo, "to" =>  "52".$celular, "username" => $username, "password" => $password)),
            CURLOPT_HTTPHEADER => array(
                "cache-control: no-cache",
                "Accept: application/json"  )
        ));
        $result = curl_exec($ch);
        $result = json_decode($result);
        return $result;
    }
}



/**
 *
//  */
class MaxcomSms {
    function enviar($codigo,$celular){

        $ch = curl_init();
        curl_setopt_array($ch, array(
            CURLOPT_URL => "https://api.broadcastermobile.com/brdcstr-endpoint-web/services/messaging/",
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_ENCODING => "",
            CURLOPT_MAXREDIRS => 10,
            CURLOPT_TIMEOUT => 30,
            CURLOPT_CUSTOMREQUEST => "POST",
            // CURLOPT_POSTFIELDS =>  http_build_query(array("from" => "dd", "message" =>  $codigo, "to" =>  "52".$celular, "username" => $username, "password" => $password)),
            CURLOPT_POSTFIELDS => "{\"apiKey\":7278,\"country\":\"MX\",\"dial\":26262,\"message\":".$codigo.",\"msisdns\":[52".$celular."],\"tag\":\"bancomerseg\"}",
            CURLOPT_HTTPHEADER => array(
                "Authorization: 17zTjV6YiTej1t4CbvVulgN0pIU=",
                "Cache-Control: no-cache",
                "Content-Type: application/json",
                "Host: api.broadcastermobile.com"
              )
        ));
        $result = curl_exec($ch);
        $err = curl_error($curl);

        if ($err) {
          echo "cURL Error #:" . $err;
        } else {
          echo $result;
        }

        // echo $result;

    }
}
