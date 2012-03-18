#NUploadFile Behavior

NUploadFile is a behavior for uploading file easily.

#Installation

Download files from <https://github.com/na3r/NUploadFile/downloads> an extract them into /protected/extensions

#Usage
###Model
in your model add NUploadFile as a behavior
<pre><code>class TestUpload extends CActiveRecord {
  public static function model($class=__CLASS__) {
		return parent::model($class);
	}
	
	public function rules() {
		return array(
			array('file', 'file', 'types'=>'jpg, png, gif, jpeg', 'on'=>'create'),
			array('file', 'file', 'allowEmpty'=>true, 'types'=>'jpg, png, gif, jpeg', 'on'=>'update'),
		);
	}

	public function behaviors() {
		return array(
				'NUploadFile'=>array(
					'class'=>'ext.NUploadFile',
					'fileField'=>'file',
				)
		);
	}	
}
</code></pre>

Note: you must set fileField property.

##controller
<pre><code>public function actionUpload($id) {
  $model = new TestUpload;
	if(isset($_POST['TestUpload'])) {
		$model->scenario = 'create';
		$model->attributes = $_POST['TestUpload'];
		if($model->validate()){
			/*
			* form is valid and we can upload file by calling UploadFile() method for uploading file
			*/
			$model->uploadFile();
			if($model->save()) {
				Yii::app()->user->setFlash('MSG', 'file was uploaded and saved into db');
				$this->refresh();
			}
		}
	}
}
</code></pre>

##Get File
i assume that the file that you'v uploaded is an image, to use file and show image we can do somthing like this
<pre><code>$model = TestUpload::model()->findByPk($id);
$url = CHtml::asset($model->getFilePath());
echo CHtml::image($url);
</code></pre>

#Comments and Suggestions
Any comment and suggestion will be accepted
