Deletable behavior helps delete many models with all relations in one shot.

* Batch deleting
* Foreign keys actions emulation (CASCADE, RESTRICT)
* Events supports
* Composite keys supports
* Transaction supports

##Requirements
Tested on Yii 1.1.10

##Usage
We have 3 models.
User HAS MANY Comment.
User HAS MANY Like.
Comment HAS MANY Like.


~~~
[php]

// User model
public function behaviors()
{
	return CMap::mergeArray(parent::behaviors(), array(
		 'deletable' => array(
			 'class' => 'ext.deletable-behavior.DeletableBehavior',
			 'relatives' => array(
				 'comments' => DeletableBehavior::CASCADE,
				 'likes'    => DeletableBehavior::CASCADE,
			 )
		 )
	));
}

// Comment model
public function behaviors()
{
	return CMap::mergeArray(parent::behaviors(), array(
		 'deletable' => array(
			 'class' => 'ext.deletable-behavior.DeletableBehavior',
			 'relatives' => array(
				 'likes'    => DeletableBehavior::CASCADE
			 )
		 )
	));
}

// Like model
public function behaviors()
{
	return CMap::mergeArray(parent::behaviors(), array(
		 'deletable' => array(
			 'class' => 'ext.deletable-behavior.DeletableBehavior',
		 )
	));
}


// event in User model
public function beforeBatchDelete($event)
{
	$batchIds = $event->sender->getBatchIds();
	foreach ($batchIds as $id)
	{
		// delete avatar img
	}
}


// all related Comments and Likes will be deleted too
User::model()->batchDelete(array(1,2,3,4)); 

~~~
Behavior must be enabled for all models, that using batch deleting.
I recommend include behavior for all models in parent model "ActiveRecord".


##Resources

 * [Project page on GitHub](https://github.com/fantgeass/deletable-behavior)
