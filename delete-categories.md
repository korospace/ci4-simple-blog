<p align="center">
    <h1 align="center">DELETE CATEGORY</h1>
    <p align="center">
        <a href="README.md"><strong>« back to home</strong></a>
    </p>
    <br />
</p>

* create button delete at table category
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
                <!-- button delete -->
                <a href="<?= base_url("/deletecategory/".$cat["id"]) ?>">
                    delete
                </a>
            </td>
        </tr>
        <?php endforeach; ?>
    </tbody>
    ```
* call your view from <b>Routes.php</b> at <b>app/Config/Routes.php</b>

    ```php
    $routes->get('/deletecategory/(:any)', 'CategoryController::deleteCategory/$1');
    ```

* create function for delete

    ```php
    class CategoryController extends BaseController
    {
        // ...

        // View Edit Category

        // delete category
        public function deleteCategory(string $id)
        {
            // delete
            $category = new Category();
            $category->where("id",$id)->delete();

            return redirect()->to(base_url('listcategory'));
        }
    }
    ```