<script>
	import { v4 } from "uuid";
	import Noty from "noty";
	import "noty/lib/noty.css";
	import "noty/lib/themes/metroui.css";
	let products = [
		{
			id: 1,
			name: "HP Pavilion Notebook",
			description: "HP Laptop",
			category: "laptop",
		},
		{
			id: 2,
			name: "Mouse Razer",
			description: "Gaming Mouse",
			category: "peripherials",
		},
	];
	let product = {
		id: "",
		name: "",
		description: "",
		category: "",
		imageURL: "",
	};

	let editStatus = false;
	const cleanProduct = () => {
		product = {
			id: "",
			name: "",
			description: "",
			category: "",
			imageURL: "",
		};
	};
	const addProduct = (e) => {
		const newProduct = {
			id: v4(),
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL,
		};
		products = products.concat(newProduct);
		cleanProduct();
		new Noty({
			theme: "metroui",
			type: "success",
			timeout: 1000,
			text: "Product Added",
		}).show();
	};

	const onSubmitHandler = (e) => {
		if (!editStatus) {
			addProduct();
		} else {
			console.log("updateProduct");
			updateProduct();
		}
	};

	const deleteProduct = (id) => {
		products = products.filter((product) => product.id !== id);
		cleanProduct();
		editStatus = false;
		new Noty({
			theme: "metroui",
			type: "warning",
			timeout: 1000,
			text: "Product Deleted Successfully",
		}).show();
	};

	const editProduct = (productEdited) => {
		product = productEdited;
		editStatus = true;
	};
	const updateProduct = () => {
		let updateProduct = {
			name: product.name,
			description: product.description,
			id: product.id,
			imageURL: product.imageURL,
			category: product.category,
		};

		const productIndex = products.findIndex((p) => p.id === product.id);
		editStatus = false;
		cleanProduct();
	};
</script>

<main>
	<div class="container">
		<div class="row">
			<div class="col-md-6">
				{#each products as product}
					<div class="card mt-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageURL}
									<img
										src="img/no-product.jpg"
										alt=""
										class="img-fluid p-2"
									/>
								{:else}
									<img
										src={product.imageURL}
										alt=""
										class="img-fluid p-2"
									/>
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
									<p class="card-text">
										{product.description}
									</p>
									<button
										on:click={editProduct(product)}
										class="btn btn-info"> Edit </button>
									<button
										on:click={deleteProduct(product.id)}
										class="btn btn-danger"> Delete </button>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<div class="form-group">
								<input
									bind:value={product.name}
									type="text"
									placeholder="Product Name"
									id="product-name"
									class="form-control"
								/>
							</div>
							<div class="form-group">
								<textarea
									bind:value={product.description}
									id="product-description"
									rows="3"
									placeholder="Product Description"
									class="form-control"
								/>
							</div>

							<div class="form-group">
								<input
									bind:value={product.imageURL}
									type="url"
									id="product-image-url"
									placeholder="https://svelte.dev/"
									class="form-control"
								/>
							</div>

							<div class="form-group">
								<select
									id="category"
									bind:value={product.category}
									class="form-control">
									<option value="laptops">Laptops</option>
									<option value="refrigeradoras"
										>Refrigeradoras</option
									>
									<option value="cocinas">Cocinas</option>
								</select>
							</div>
							<button class="btn btn-success">
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
