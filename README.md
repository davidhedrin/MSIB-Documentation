# MSIB Documentation IT Dept
![Generic badge](https://img.shields.io/badge/MSIB-IT_Detpartment-green.svg) ![Generic badge](https://img.shields.io/badge/MSIB-Batch_6-red.svg)

## Vee-Validate
Vee-validate for validation from client server. https://vee-validate.logaretm.com/v4/
> Download and install library needed inside your vue project:

```
// Download vee-validate
$ npm i vee-validate

// Download yup library for customize validation
$ npm i yup
```

> In your file .vue page, import vee-validate and yup library and call that to var components inside *export default* script tag. For example **Home.vue**
```
<script>
    import { Form, Field } from 'vee-validate';
    import * as Yup from 'yup';
    
    export default {
        components: { Form, Field },
        // Your logic here
    }
</script>
```

> After import libary inside script tag your page .vue. Let's configure your validation required inside the **data()** function of your script tag.

```
// export default of your script tag Home.vue
export default {
    data(){
        // Create config for validate form login
        const validateFormLogin = Yup.object().shape({
          email: Yup.string().email('Format email tidak sesuai').required('Field email wajib diisi'),
          password: Yup.string().required('Field password wajib diisi').min(6, 'Password minimal 6 karakter')
        });
        
        // Create config for validate form register
        const validateFormRegister = Yup.object().shape({
          username: Yup.string().required('Field username wajib diisi').min(6, 'Password minimal 6 karakter'),
          email: Yup.string().email('Format email tidak sesuai').required('Field email wajib diisi'),
          password: Yup.string().required('Field password wajib diisi').min(6, 'Password minimal 6 karakter').oneOf([Yup.ref('co_password')], 'Password dan konformasi password harus sama'),
          co_password: Yup.string().required('Field co-password wajib diisi').min(6, 'Co-password minimal 6 karakter').oneOf([Yup.ref('password')], 'Password dan konformasi password harus sama')
        });

        // After setting config, call your Variable validate in the return of function data()
        return {
          validateFormLogin, // Var validate of login
          dataUserLogin: {
            email: null,
            password: null,
          },
          
          validateFormRegister, // Var validate of login
          dataUserRegister: {
            username: null,
            email: null,
            password: null,
            co_password: null,
          },
        };
    },
}
```
> After finish configure your validation required in script tag of your page. Now, call your config validation and your var of model in to Form and Field tag in your document html in tamplate tag.
- **Login form**
```
// Login form
<Form @submit="loginUser()" ref="formLoginRef" :validation-schema="validateFormLogin" v-slot="{ errors }">
    <div class="form-group mb-2">
      <label class="mb-0" for="email_address_field">Email</label>
      <Field type="email" v-model="dataUserLogin.email" id="email_address_field" name="email" class="form-control px-2" :class="{'is-invalid' : errors.email}" placeholder="email@example.com" />
      <div class="text-danger fs--1">{{ errors.email }}</div>
    </div>
    <div class="form-group mb-3">
      <label class="mb-0" for="password_field">Password</label>
      <Field type="password" v-model="dataUserLogin.password" id="password_field" name="password" class="form-control px-2" :class="{'is-invalid' : errors.password}" placeholder="*********" />
      <div class="text-danger fs--1">{{ errors.password }}</div>
    </div>
    
    <div class="mb-3">
      <button class="btn btn-sm btn-danger hover-top btn-glow" type="submit">
        Masuk <i class='bx bx-log-in-circle'></i>
      </button>
    </div>
</Form>
```
- **Register form**
```
<Form ref="formRegisterRef" @submit="storeNewUser()" :validation-schema="validateFormRegister" v-slot="{ errors, resetForm }">
    <div class="form-group mb-2">
      <label class="mb-0">Username</label>
      <Field v-model="dataUserRegister.username" type="text" name="username" class="form-control px-2" :class="{'is-invalid' : errors.username}" placeholder="Masukkan username" />
      <div class="text-danger fs--1">{{ errors.username }}</div>
    </div>
    <div class="form-group mb-2">
      <label class="mb-0">Email</label>
      <Field v-model="dataUserRegister.email" type="email" name="email" class="form-control px-2" :class="{'is-invalid' : errors.email}" placeholder="email@example.com" />
      <div class="text-danger fs--1">{{ errors.email }}</div>
    </div>
    <div class="form-group mb-2">
      <label class="mb-0">Password</label>
      <Field v-model="dataUserRegister.password" type="password" name="password" class="form-control px-2" :class="{'is-invalid' : errors.password}" placeholder="*********" />
      <div class="text-danger fs--1">{{ errors.password }}</div>
    </div>
    <div class="form-group mb-3">
      <label class="mb-0">Konfirmasi Password</label>
      <Field v-model="dataUserRegister.co_password" type="password" name="co_password" class="form-control px-2" :class="{'is-invalid' : errors.co_password}" placeholder="*********" />
      <div class="text-danger fs--1">{{ errors.co_password }}</div>
    </div>
    
    <div class="mb-3">
      <button class="btn btn-sm btn-danger hover-top btn-glow" type="submit">
        Register
      </button>
    </div>
</Form>
```

**Well Done, Happy hacker!**
