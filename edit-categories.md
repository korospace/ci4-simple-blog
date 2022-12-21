<p align="center">
    <h1 align="center">EDIT CATEGORY</h1>
    <p align="center">
        <a href="../README.md"><strong>« back to home</strong></a>
    </p>
    <br />
</p>

* create button edit at table category
    ```html
    <tbody>
    <?php $i=1; ?>
    <?php foreach($categories as $cat) : ?>
    <tr>
        <th scope="row">
            <?= $i++ ?>
        </th>
        <td>
            <?= $cat['name'] ?>
        </td>
        <td>
            <!-- button edit -->
            <a href="<?= base_url("/formeditcategory/".$cat["id"]) ?>">
                edit
            </a>
        </td>
    </tr>
    <?php endforeach; ?>
</tbody>
    ```

* create view <b>EditCategory.php</b> at folder <b>app/Views/Category</b>

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Edit Category</title>

        <!-- Bootstrap-5 css -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

    </head>
    <body>

        <h1>Edit Category</h1>
        
        <!-- Bootstrap-5 js -->
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
    </body>
    </html>
    ```
* call your view from <b>Routes.php</b> at <b>app/Config/Routes.php</b>

    ```php
    $routes->get('/formeditcategory/(:any)', 'CategoryController::viewEditCategory/$1');
    ```

* create function for call your view

    ```php
    class CategoryController extends BaseController
    {
        // ...

        // View List Category

        // View Edit Category
        public function viewEditCategory(string $id)
        {
            return view('/Category/EditCategory.php');
        }
    }
    ```

* get detail category and pass to view

    ```php
    class CategoryController extends BaseController
    {
        // ...
        
        // View List Category

        // View Edit Category
        public function viewEditCategory(string $id)
        {
            // get data
            $category = new Category();

            $data = [
                'category' => $category->where('id',$id)->first()
            ];

            return view('/Category/EditCategory.php',$data);
        }
    }
    ```

* modify your view

    ```html
    <div class="container">
        <h1>EDIT CATEGORY</h1>

        <!-- flash data -->
        <?php if (session()->getFlashdata('success')) { ?>
            <div class="alert alert-success alert-dismissible fade show" role="alert">
                <?= session()->getFlashdata('success') ?>
                <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
        <? } ?>

        <form action="<?= base_url('/editcategory') ?>" method="post" class="row mt-3">

            <input type="hidden" name="category_id" value="<?= $category['id'] ?>">
            
            <div class="col-5">

                <div>
                    <label for="category_name" class="form-label">Category Name</label>
                    <input id="category_name" class="form-control" type="text" name="category_name" value="<?= $category['name'] ?>">

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
    </div>
    ```

* add route for recive data
    ```php
    $routes->put('/editcategory', 'CategoryController::editCategory');
    ```

* create function for recive data

    ```php

    class CategoryController extends BaseController
    {
        // ...

        // View Edit Category

        // Edit Category
        public function editCategory()
        {
            // validation
            $rules = [
                "category_name" => "required|max_length[255]",
            ];

            if (!$this->validate($rules)) {
                return view('/Category/FormEditCategory.php', [
                    "validation" => $this->validator,
                ]);
            }

            $id = $this->request->getPost('category_id');
            $data = [
                'name' => $this->request->getPost('category_name')
            ];

            // update
            $category = new Category();
            $category->update($id,$data);

            // redirect
            session()->setFlashdata("success", "Kategori berhasil di update");

            return redirect()->to(base_url('formeditcategory/'.$id));
        }

    }
    ```