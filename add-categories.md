<p align="center">
    <h1 align="center">ADD CATEGORY</h1>
    <p align="center">
        <a href="README.md"><strong>« back to home</strong></a>
    </p>
    <br />
</p>

<details open="open">
  <summary>List Content</summary>
  <ul>
    <li>
        <a href="#create-table-category">
            Create Table Category
        </a>
    </li>
    <li>
        <a href="#create-form-insert-category">
            Create Form Insert Category
        </a>
    </li>
    <li>
        <a href="#send-data">
            Send Data
        </a>
    </li>
    <li>
        <a href="#insert-into-table">
            Insert Into Table
        </a>
    </li>
    <li>
        <a href="#validation">
            Validation
        </a>
    </li>
  </ul>
</details>

## Create Table Category
* create file migration category
    ```
    php spark make:migration Category
    ```
* define table category
    ```php
    class Category extends Migration
    {
        public function up()
        {
            $this->forge->addField([
                'id' => [
                    'type'       => 'int',
                    'constraint' => 11,
                    'auto_increment' => true,  
                ],
                'name' => [
                    'type'       => 'varchar',
                    'constraint' => 255,
                    'unique'     => true,
                    'null'       => false,
                ],
            ]);

            $this->forge->addPrimaryKey('id');
            $this->forge->createTable('categories');
        }

        public function down()
        {
            $this->forge->dropTable('categories');
        }
    }
    ```
* run file migration
    ```
    php spark migrate
    ```

## Create Form Insert Category

* create view <b>AddCategory.php</b> at folder <b>app/Views/Category</b>

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Add Category</title>

        <!-- Bootstrap-5 css -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

    </head>
    <body>

        <h1>ADD CATEGORY</h1>

        <!-- Bootstrap-5 js -->
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
    </body>
    </html>
    ```
* call your view from <b>Routes.php</b> at <b>app/Config/Routes.php</b>

    ```php
    // cara 1
    $routes->get('/addcategory', function () {
        return view('/Category/AddCategory.php');
    });

    // cara 2
    $routes->get('/addcategory', 'CategoryController::viewAddCategory');
    ```

* create controller Category</b>

    ```sh
    php spark make:controller CategoryController
    ```
    _NOTE_ : this action will generate your controller at folder <b>app/Controller</b>

* create function for call your view

    ```php
    class CategoryController extends BaseController
    {
        // View Add Category
        public function viewAddCategory()
        {
            return view('/Category/AddCategory.php');
        }
    }
    ```

* modify your view

    ```html
    <div class="container">
        <h1>ADD CATEGORY</h1>

        <!-- form -->
        <form action="" class="row mt-3">
            
            <div class="col-5">
                <div>
                    <label for="category_name" class="form-label">Category Name</label>
                    <input id="category_name" class="form-control" type="text" name="category_name">
                </div>
    
                <button class="btn btn-primary w-100 mt-3">
                    simpan
                </button>
            </div>

        </form>
    </div>
    ```
<br>

## Send Data

* setup your data destination

    ```html
    <form action="<?= base_url('/addcategory') ?>" method="post" class="row mt-3">

    </form>
    ```

* add route for recive data
    ```php
    $routes->post('/addcategory', 'CategoryController::addCategory');
    ```

* create function for recive data

    ```php
    class CategoryController extends BaseController
    {
        // View Add Category

        // Add Category
        public function addCategory()
        {
            $data = [
                'name' => $this->request->getPost('category_name')
            ];
        
            dd($data);
        }

    }
    ```

## Insert into table

* create model <b>Category</b>

    ```
    php spark make:model Category
    ```

* setup your model>

    ```php
    class Category extends Model
    {
        protected $table            = 'categories';
        protected $allowedFields    = ['name'];
    }
    ```

* insert into table

    ```php
    class CategoryController extends BaseController
    {
        // View Add Category

        // Add Category
        public function addCategory()
        {
            $data = [
                'name' => $this->request->getPost('category_name')
            ];

            // insert
            $category = new Category();
            $category->save($data);

            // redirect
            session()->setFlashdata(
                "success", 
                "Kategori berhasil di tambahkan"
            );

            return redirect()->to(base_url('formaddcategory'));
        }
    }
    ```

* recive flash data at view form category
    ```html
    <?php if (session()->getFlashdata('success')) { ?>
        <div class="alert alert-successs alert-dismissible fade show" role="alert">
            <?= session()->getFlashdata('success') ?>
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
    <? } ?>
    ```

## Validation

* add validation rules
    ```php
    class CategoryController extends BaseController
    {
        // View Add Category

        // Add Category
        public function addCategory()
        {
            // validation
            $rules = [
                "category_name" => "required|max_length[255]",
            ];

            if (!$this->validate($rules)) {
                return view('/Category/FormAddCategory.php', [
                    "validation" => $this->validator,
                ]);
            }

            $data = [
                'name' => $this->request->getPost('category_name')
            ];

            // insert
            $category = new Category();
            $category->save($data);

            // redirect
            session()->setFlashdata(
                "success", 
                "Kategori berhasil di tambahkan"
            );

            return redirect()->to(base_url('formaddcategory'));
        }
    }
    ```

* recive validation error
    ```html
    <form action="<?= base_url('/addcategory') ?>" method="post" class="row mt-3">
            
        <div class="col-5">
            <div>
                <label for="category_name" class="form-label">Category Name</label>
                <input id="category_name" class="form-control" type="text" name="category_name">

                <!-- recive error -->
                <?php if (isset($validation)) { ?>
                    <small class="text-danger mt-2">
                        <?= $validation->getError('category_name') ?>
                    </small>
                <?php } ?>
            </div>

            <button class="btn btn-primary w-100 mt-3">
                simpan
            </button>
        </div>

    </form>
    ```