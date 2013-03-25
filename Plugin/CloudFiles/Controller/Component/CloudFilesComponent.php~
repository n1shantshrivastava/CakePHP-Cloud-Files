<?php
App::import('Vendor', 'php-cloud-files/cloudfiles'); //, array('file' => 'cloudfiles.php'));

class CloudFilesComponent extends Object {

    protected $userName = 'RACKSPACE_USER_NAME';
    protected $apiKeyCF = 'RACKSPACE_API_KEY';
    public $containerName = 'CLOUD_FILE_CONTAINER_NAME';
    public $authenticateToCloud = null;
    public $connectToCloud = null;
    public $container = null;


    public function initialize($controller) {
        try {
            $this->authenticateToCloud = new CF_Authentication($this->userName, $this->apiKeyCF, null, UK_AUTHURL);
            $this->authenticateToCloud->authenticate();

            $this->connectToCloud = new CF_Connection($this->authenticateToCloud);
        } catch (Exception $e) {
            echo 'Caught exception : ', $e->getMessage(), "\n";
        }
    }

    public function startup() {
    }

    public function beforeRender() {
    }

    public function shutdown() {
    }

    public function beforeRedirect() {
    }

    public function createContainer($containerName = null) {
        if (!empty($containerName)) {
            try {
                $this->container = $this->connectToCloud->create_container($containerName);
                return $this->container;
            } catch (NoSuchContainerException $e) {
                echo 'Caught exception : ', $e->getMessage(), "\n";
            }
        }
        return false;
    }

    public function moveFileToCloud($filePath = null, $fileName = null) {

        if (!empty($fileName) && !empty($filePath)) {
            $return = array('error'   => true,
                            'message' => 'can not upload file');
            try {
                $this->container = $this->getContainerInstance($this->containerName);
                $obj = $this->container->create_object($fileName);
                $obj->load_from_filename($filePath);

                $return['public_uri'] = $obj->public_uri();
                $return['error']      = false;
                $return['message']    = '';

            } catch (NoSuchObjectException $e) {
                $return['public_uri'] = '';
                $return['error']      = true;
                $return['message']    = 'true exception : ' . $e->getMessage() . "\n";

            }
            return $return;
        }
        return false;
    }

    public function getContainerInstance($containerName = null) {
        try {
            return $this->connectToCloud->get_container($containerName);
        } catch (Exception $e) {
            echo 'Caught exception : ', $e->getMessage(), "\n";
        }
    }

    public function setContainerName($containerName) {
        try {
            $this->containerName = $containerName;
        } catch (Exception $e) {
            echo 'Caught exception : ', $e->getMessage(), "\n";
        }
    }
}
