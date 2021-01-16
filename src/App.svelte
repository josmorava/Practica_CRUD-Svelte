<script>
	//modulo para la creación de id's únicos
	import {v4} from 'uuid';
	//modulo para la muestra de notificaciones
	import Noty from 'noty';

	import 	'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css';



	let products = [
		{
			id: 1,
			name: 'HP Pavilon Netobook',
			description: 'HP laptop',
			category: 'laptop'
		},{
			id: 2,
			name: 'Mouse Raizer',
			description: 'Gamer Mouse',
			category: 'peripherials'
		}
	];

	//constanate para guardar los productos
	let product = {
		id:'',
		name:'',
		description: '',
		category:'',
		imageURL:''
	};

	let editStatus = false;

	const cleanProduct = () => {
		product = {
			id: '',
			name: '',
			description: '',
			category: '',
			imageURL: ''
		}
	}

	const addProduct = () =>{
		const newProduct = {
			id: v4(),
			name : product.name,
			description : product.description,
			category: product.category,
			imageURL: product.imageURL
		} 

		//concatenando el producto capturado al array de products
		products = products.concat(newProduct);

		//luego de añadir el producto se llama la funcion que limpia el formulario 
		cleanProduct();
	}

	const updateProduct = () =>{
		let updatedProduct = {
			id: product.id,
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL
		}

		//buscando si el id del prosucto está en la lista de productos agregados
		const productIndex = products.findIndex(p => p.id === product.id)

		//busca dentro del arreglo de productos el indice del producto que va a actualizar y ejecuta la funcion de actualizar
		products[productIndex] = updatedProduct;
		cleanProduct();

		//luego de actualizar el producto vuelve al estado de editar falso
		editStatus = false;
				//Valores a mostrar para la notificacion
				new Noty ({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			text: 'Product Update Succesfully'
		}).show();
	}

	const onSubmitHandler = e => {
		if(!editStatus){
			addProduct();
		}	else {
			updateProduct();
		}	
	}

	const deleteProduct = (id) => {
		//crea un nuevo arreglo donde filtra los elementpos que son diferentes al id que selecciona, crea un arreglo menos el id seleccionado
		products = products.filter(product => product.id !== id);
	}

	const editProduct = productEdited => {
		product = productEdited;
		editStatus = true;
	}
	
</script>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6">

				<!--iterador entre los productos
					hace que por cada producto se muestre
					un card-->
				{#each products as product}
					<div class="card mt-6 mt-3">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageURL}
									<img 
										src="images/no-product.png" 
										alt=""
										class="img-fluid p-2">
								{:else}
									<img 
										src={product.imageURL}
										alt=""
										class="img-fluid p-2">
								{/if}
								
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5>
										<strong>
											{product.name}
										</strong>
										<span>
											<small>
												{product.category}
											</small>	
										</span>
									</h5>	
									<p class="casrd-text">
										{product.description}
									</p>	
									<button 
										class="btn btn-danger"
										on:click={deleteProduct(product.id)}>
										Delete
									</button>
									<button 
										on:click={editProduct(product)}
										class="btn btn-secondary">
										Edit
									</button>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>


			<div class="col-md-6 mt-3">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<!--forma de enlazar valores a un objeto-->
							<div class="form-group mb-3">
								<input 
								bind:value={product.name} 
								type="text" 
								placeholder="Produt Name" 
								id="product-name" 
								class="form-control"
								/>
							</div>

							<div class="form-group mb-3">
								<textarea 
								bind:value={product.description}
								placeholder="Product Description"  id="product-description"  
								rows="3"
								class="form-control" />	
							</div>

							<div class="form-group mb-3">
								<input 
								bind:value={product.imageURL}
								type="url" 
								id="product-img-url" 
								placeholder="https://facebook.com" 
								class="form-control"/>
							</div>

							<div class="form-group mb-3">
								<!--Etiqueta para seleccionar un tipo de elemento / nueva -->
								<select 
									bind:value={product.category}  
									id="category"
									class="form-control">
									<option value="laptops">Laptops</option>
									<option value="peripherials">Peripherials</option>
									<option value="servers">Servers</option>
								</select>
							</div>
							
							<button class="btn btn-primary">
								{#if !editStatus}
									Save Product
								{:else}
									Update Product
								{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>

</style>