<p align="center">
    <h1 align="center">LIST CATEGORY</h1>
    <p align="center">
        <a href="README.md"><strong>« back to home</strong></a>
    </p>
    <br />
</p>

* create view <b>ListCategory.php</b> at folder <b>app/Views/Category</b>

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>List Category</title>

        <!-- Bootstrap-5 css -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

    </head>
    <body>

        <h1>List Category</h1>
        
        <!-- Bootstrap-5 js -->
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
    </body>
    </html>
    ```
* call your view from <b>Routes.php</b> at <b>app/Config/Routes.php</b>

    ```php
    $routes->get('/listcategory', 'CategoryController::viewListCategory');
    ```
* create function for call your view

    ```php
    class CategoryController extends BaseController
    {
        // View Add Category

        // Add Category

        // View List Category
        public function viewListCategory()
        {
            return view('/Category/ListCategory.php');
        }
    }
    ```
* get data from database

    ```php
    class CategoryController extends BaseController
    {
        // View Add Category

        // Add Category

        // View List Category
        public function viewListCategory()
        {
            // get data
            $category = new Category();
            
            $data = [
                'categories' => $category->findAll()
            ];

            // passing to view
            return view('/Category/ListCategory.php',$data);
        }
    }
    ```
* display data on html table
    ```html
    <div class="container">
        <h1>List Category</h1>

        <table class="table mt-4">
            <thead>
                <tr>
                <th scope="col">#</th>
                <th scope="col">NAME</th>
                <th scope="col">ACTION</th>
                </tr>
            </thead>
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
                        edit
                    </td>
                </tr>
                <?php endforeach; ?>
            </tbody>
        </table>
    </div>
    ```