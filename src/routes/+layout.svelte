<script>
	
// Your selected Skeleton theme:
import '@skeletonlabs/skeleton/themes/theme-skeleton.css';

// This contains the bulk of Skeletons required styles:
import '@skeletonlabs/skeleton/styles/skeleton.css';

// Finally, your application's global stylesheet (sometimes labeled 'app.css')
import '../app.postcss';

</script>
  
  <div class="navbar">
	<nav class="container md:items-center">
		<ul>
			<li>
				<div class="flex items-center justify-between">
					<a class="text-xl font-bold hover:text-blue-400" href="/ProfilesList">Profiles</a>
				</div>
			</li>

			<li>
				<div class="flex items-center justify-between">
					<a class="text-xl font-bold hover:text-blue-400" href="/Add">Add Profile</a>
				</div>
			</li>
        </ul>
	</nav>
</div>

<style>


	.navbar {
		padding: 10px;

		display: flex;

		justify-content: space-between;

		align-items: center;

		background-color: #fbf9f4;

		color: rgb(90 90 90);
	}

	.navbar ul {
		list-style: none;

		display: flex;

		gap: 10px;
	}

	.navbar ul li {
		padding: 5px;
	}
</style>

<slot />
