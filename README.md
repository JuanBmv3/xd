@model List<WebApplication1.Models.Peliculas>

@{
    ViewBag.Title = "About";
}

<style type="text/css">
    .image_container {
        height: 120px;
        width: 200px;
        border-radius: 6px;
        overflow: hidden;
        margin: 10px;
    }

        .image_container img {
            height: 100%;
            width: auto;
            object-fit: cover;
        }

        .image_container span {
            top: -6px;
            right: 8px;
            color: red;
            font-size: 28px;
            font-weight: normal;
            cursor: pointer;
        }
</style>

<h2>@ViewBag.Title.</h2>
<h3>@ViewBag.Message</h3>

<p>Use this area to provide additional information.</p>

<div class="container mt-3 w-100">
    <div class="card shadow-sm w-100">
        <div class="card-header d-flex justify-content-between">
            <h4>Image Uploading</h4>
            <form class="form" action="#" method="post" id="form">
                <input type="file" name="Image" id="image" multiple="multiple" class="d-none" onchange="image_select()">
                <button class="btn btn-sm btn-primary" type="button" onclick="document.getElementById('image').click()">Choose Images</button>
            </form>
        </div>
        <div class="card-body d-flex flex-wrap justify-content-start" id="container">
            <!-- Image will be show here-->
        </div>
    </div>
</div>

<table class="table">
    <thead>
        <tr>
            <th>Titulo</th>
            <th>Duracion</th>
            <th>Pais</th>
            <th>Lanzamiento</th>

        </tr>
    </thead>
    <tbody>
        @foreach (var pelicula in Model)
        {
            <tr>
                <td>@pelicula.Titulo</td>
                <td>@pelicula.Duracion</td>
                <td>@pelicula.Pais</td>
                <td>@pelicula.Publicacion.ToString("dd/MM/yyyyy")</td>
            </tr>
        }
    </tbody>
</table>

<script>

    // this variable will store images for preview
    var images = [];

    // this function will select images
    function image_select() {
        var image = document.getElementById('image').files;
        for (i = 0; i < image.length; i++) {
            if (check_duplicate(image[i].name)) {
                images.push({
                    "name": image[i].name,
                    "url": URL.createObjectURL(image[i]),
                    "file": image[i],
                })

                console.log(images);
            } else {
                alert(image[i].name + " is already added to the list");
            }
        }

        document.getElementById('form').reset();
        document.getElementById('container').innerHTML = image_show();
    }

    // this function will show images in the DOM
    function image_show() {
        var image = "";
        images.forEach((i) => {
            image += `<div class="image_container d-flex justify-content-center position-relative">
                   <img src="`+ i.url +`" alt="Image">
                   <button class="xd" id=`+ images.indexOf(i) + ` onclick="delete_image(`+ images.indexOf(i) + `)">&times;</button>

              </div>`;
        })
        return image;
    }

    // this function will get all images from the array
    function get_image_data() {
        var form = new FormData()
        for (let index = 0; index < images.length; index++) {
            form.append("file[" + index + "]", images[index]['file']);
        }
        return form;
    }

    // this function will delete a specific image from the container
    function delete_image() {
        var test = document.querySelectorAll('.xd')
        //images.splice(1, i);
        console.log(test.id);
        document.getElementById('container').innerHTML = image_show();
    }


    // this function will check duplicate images
    function check_duplicate() {
        var image = true;
        if (images.length > 0) {
            for (e = 0; e < images.length; e++) {
                if (images[e].name == name) {
                    image = false;
                    break;
                }
            }
        }
        return image;
    }
 </script>
