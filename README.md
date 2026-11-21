
<!DOCTYPE html>
<html>
<head>
    <title>AJAX User Insert</title>

    <meta name="csrf-token" content="{{ csrf_token() }}">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
</head>

<body class="bg-light">

<div class="container mt-5">
    <div class="card shadow p-4">

        <h4 class="mb-3">User Registration</h4>

        <form id="userForm" action="{{ route('user.store') }}" method="POST">

            @csrf

            <div class="mb-3">
                <label>Name</label>
                <input type="text" name="name" class="form-control">
                <span class="text-danger error-name"></span>
            </div>

            <div class="mb-3">
                <label>Email</label>
                <input type="email" name="email" class="form-control">
                <span class="text-danger error-email"></span>
            </div>

            <div class="mb-3">
                <label>Password</label>
                <input type="password" name="password" class="form-control">
                <span class="text-danger error-password"></span>
            </div>

            <button type="submit" class="btn btn-primary">
                Submit
            </button>

        </form>

    </div>
</div>

<script>

$(document).ready(function () {

    // CSRF Token
    $.ajaxSetup({
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
        }
    });

    $('#userForm').submit(function (e) {

        e.preventDefault();

        // Clear old errors
        $('.text-danger').text('');

        let form = $(this);

        $.ajax({

            url: form.attr('action'),

            type: 'POST',

            data: form.serialize(),

            dataType: 'json',

            success: function (response) {

                if (response.status == true) {

                    Swal.fire({
                        icon: 'success',
                        title: 'Success!',
                        text: response.message
                    });

                    form[0].reset();
                }
            },

            error: function (xhr) {

                if (xhr.status === 422) {

                    let errors = xhr.responseJSON.errors;

                    $.each(errors, function (key, value) {

                        $('.error-' + key).text(value[0]);

                    });

                } else {

                    Swal.fire({
                        icon: 'error',
                        title: 'Error!',
                        text: xhr.responseJSON?.message || 'Something went wrong!'
                    });

                }
            }

        });

    });

});

</script>

</body>
</html>
