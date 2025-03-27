### Создание админ-панели в Yii2

#### Шаг 1: Создание контроллера AdminController

1. Копируем любой контроллер и переименовываем его в `AdminController.php`:
2. Добавим функцию `beforeAction` - beforeAction выполняется каждый раз при отправке запроса на /admin
   
```php
class AdminController extends Controller
{
    /**
     * @inheritDoc
     */
    public function behaviors()
    {
        return array_merge(
            parent::behaviors(),
            [
                'verbs' => [
                    'class' => VerbFilter::className(),
                    'actions' => [
                        'delete' => ['POST'],
                    ],
                ],
            ]
        );
    }

    public function beforeAction($action)
    {
        if (!parent::beforeAction($action)) {
            return false;
        }

        // Дополнительная проверка перед каждым действием на роль. Если роль не Админ - редирект на главную страницу
        if (Yii::$app->user->isGuest || Yii::$app->user->identity->role_id !== 2) {
            return $this->redirect(['site/index']);
        }

        return true;
    }

    /**
     * Lists all Role models.
     *
     * @return string
     */
    public function actionIndex()
    {
        
        return $this->render('index');
    }

}

```

#### Шаг 2: Создание представлений для админ-панели

1. Создаем папку `views/admin`
2. Создаем файл `views/admin/index.php`:

```php
<?php
use yii\helpers\Html;

/* @var $this yii\web\View */

$this->title = 'Админ-панель';
?>
<div class="admin-index">
    <h1><?= Html::encode($this->title) ?></h1>

    <div class="list-group">
        <?= Html::a('Управление заявками', ['request/index'], ['class' => 'list-group-item list-group-item-action']) ?>
    </div>
</div>
```


#### Шаг 3: Проверка работы админ-панели

1. Зарегистрируйте нового пользователя и измените его роль в базе данных на администратора
2. Войдите в систему под учетной записью администратора
3. Перейдите по URL адресу `/admin`
4. Убедитесь, что:
   - Отображается главная страница админ-панели
   - Ссылка на заявки работает
   - При попытке доступа не-администратором происходит перенаправление

#### Особенности реализации:

 **Проверка прав**:
   - Проверка в `beforeAction`
   - Проверка по role_id 



### [Следующая лекция](https://github.com/petrocollege-web/7.-Menu-and-content)
### [Предыдущая лекция](https://github.com/petrocollege-web/5.-Login/)
