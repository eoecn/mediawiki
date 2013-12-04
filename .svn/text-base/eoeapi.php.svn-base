<?php

$eoe = new eoecache($_GET['page']);
echo $eoe->show();

class eoecache
{
	var $page;
	var $filehash;
	var $cachefile;
	
	function __construct($page)
	{
		$this->page = $page;
		$this->filehash = md5($page);
		$this->cachefile = "eoeapi/{$this->filehash}.html";
	}
	
	function show(){
		if(empty($this->page))
			return false;
		if($this->status())
			return $this->get();
		else
			return $this->set();
	}
	
	function status()
	{
		if(!file_exists($this->cachefile))
			return false;
		
		//@TODO 添加过期时间
		
		return true;
	}
	
	function set()
	{
		$url = "http://wiki.eoeandroid.com/api.php?action=parse&format=json&page=".$this->page;
		$data = file_get_contents( $url );
		$data = json_decode($data);
		
		$text = (array)$data->parse->text;
		$text = $this->format($text['*']);
		$title = $data->parse->title;
		
		$data = array("title"=>$title, "text"=>trim($text));
		$data = json_encode($data);
		file_put_contents($this->cachefile, $data);
		
		return $this->get();
	}
	
	function get()
	{
		$data = file_get_contents($this->cachefile);
		if(!empty($_GET['callback']))
		{
			return sprintf("%s(%s)", $_GET['callback'], $data);
		}else{
			return $data;
		}
	}
	
	function format($data){
		$filter = array(
				'<!\-\-[\w\W]*?\-\->'								=>'',
				'<span class="editsection">.*?</span>'				=>'',
				'href="/'											=>'href="http://wiki.eoeandroid.com/',
				'src="/'											=>'src="http://wiki.eoeandroid.com/',
				'http://developer.android.com'						=>'http://docs.eoeandroid.com',
		);
		foreach ($filter as $match=>$replace)
			$data = preg_replace("@{$match}@is", $replace, $data);
	
		return $data;
	}
}