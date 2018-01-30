<?php
namespace v1;

use core\ErrorCode;

class Restful
{
    /**
     * 用户类
     * @var User
     */
    private $_user;
    /**
     * 文章类
     * @var Article
     */
    private $_article;

    /**
     * 允许请求的资源列表
     * @var array
     */
    private $_allowResource = [
        'users',
        'articles'
    ];

    /**
     * 允许请求的方式
     * @var array
     */
    private $_allowMethod = [
        'GET',
        'POST',
        'PUT',
        'DELETE',
        'OPTIONS'
    ];

    /**
     * 请求方法
     * @var
     */
    private $_requestMethod;

    /**
     * 资源名称
     * @var
     */
    private $_resourceName ;

    /**
     * 请求id
     * @var
     */
    private $_id ;

    /**
     * 常用的状态码及其说明
     * @var array
     */
    private $_statusCode = [
        200 => 'OK',
        204 => 'No Content',
        400 => 'Bad Request',
        401 => 'Unauthorized',
        403 => 'Forbidden',
        404 => 'Not Found',
        405 => 'Method Not Allowed',
        500 => 'Server Internal Error'
    ];

    /**
     * 构造方法
     * Restful constructor.
     * @param User $_user
     * @param Article $_article
     */
    public function __construct(User $_user, Article $_article)
    {
        $this->_user = $_user;
        $this->_article = $_article;
    }

    /**
     * 公共入库
     */
    public function run()
    {
        try {
            $this->_setupRequestMethod();
            $this->_setupResource();
            //处理user
            if($this->_resourceName == 'users') {
                return $this->_json($this->_handelUsers());
                //处理 article
            } elseif($this->_resourceName == 'articles') {
                return $this->_json($this->_handelArticles());
            }
        }catch (\Exception $e) {
            $this->_json(['error' => $e->getMessage()] , $e->getCode());
        }

    }


    /**
     * 初始化请求资源
     * @throws \Exception
     * @internal param mixed $resourceName
     */
    private function _setupResource()
    {
        $path =  $_SERVER['PATH_INFO'];
        $resource = explode('/',  $path);
        $this->_resourceName = $resource[1];
        if(!in_array($this->_resourceName, $this->_allowResource)) {
            throw new \Exception('请求方法不被允许', 400);
        }
        if(!empty($resource[2])) {
            $this->_id = $resource[2];
        }
    }

    /**
     * 初始化请求方法
     */
    private function _setupRequestMethod()
    {
        $this->_requestMethod = $_SERVER['REQUEST_METHOD'];
        if(!in_array($this->_requestMethod, $this->_allowMethod)) {
            throw new \Exception('请求方法不被允许',  405);
        }
    }

    /**
     * 把数据转换成json并返回给前台
     * @param $array
     * @param int $code
     */
    private function _json($array, $code = 0) {
        //如果错误码大于0 并且不是200 表示出错 需要讲错误码显示出来
        /*if($code > 0 && $code != 200 && $code != 204) {
            header("HTTP/1.1".$code." ". $this->_statusCode[$code]);
        }*/
        //输出类型为json 标码为utf-8
        header('Content-Type: application/json; charset=utf-8');
        echo json_encode($array, JSON_UNESCAPED_UNICODE);
        exit();
    }

    /**
     * 获取请求体参数
     * @return mixed
     * @throws \Exception
     */
    private function _getBodyParams()
    {

        $data = file_get_contents('php://input');
        if(empty($data)) {
            throw  new \Exception('请求参数错误', 400);
        }
        //解析成数组返回
        return json_decode($data, true);
    }


}