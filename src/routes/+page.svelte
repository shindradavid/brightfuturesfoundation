<script lang="ts">
	import type { EmblaCarouselType } from 'embla-carousel';
	import emblaCarouselSvelte from 'embla-carousel-svelte';
	import Autoplay from 'embla-carousel-autoplay';

	import { TextField } from '$lib/components/form-fields';

	const options = { loop: true };
	const plugins = [Autoplay()];

	let emblaApi: EmblaCarouselType;

	function onInit(event: CustomEvent<EmblaCarouselType>): void {
		emblaApi = event.detail;
	}

	const heroBgImages = [
		'/images/img-007.png',
		'/images/img-006.png',
		'/images/img-002.png',
		'/images/img-004.png'
	];

	let amount = $state('');
	let name = $state('');
	let email = $state('');
	let selectedAmount = $state<'custom' | '$10' | '$50' | '$100'>('custom');
</script>

<main>
	<section id="hero" class="hero">
		<!-- overlay -->
		<div class="hero__overlay"></div>
		<!-- content -->
		<div class="hero__content">
			<div class="copy">
				<h1 class="heading">
					Creating Lasting Change and Brighter Futures for Communities in Uganda.
				</h1>

				<p class="subheading">
					Together, we are creating opportunities for education, healthcare, and sustainable
					development in Uganda.
				</p>

				<div class="cta">
					<a href="/about-us" class="btn btn-primary">Learn more</a>
				</div>
			</div>

			<div class="form-container">
				<form class="form">
					<h2>DONATE</h2>
					<div class="btns">
						<button
							onclick={() => (selectedAmount = '$10')}
							class:selected={selectedAmount === '$10'}
						>
							$10
						</button>
						<button
							onclick={() => (selectedAmount = '$50')}
							class:selected={selectedAmount === '$50'}
						>
							$50
						</button>
						<button
							onclick={() => (selectedAmount = '$100')}
							class:selected={selectedAmount === '$100'}
						>
							$100
						</button>
						<button
							onclick={() => (selectedAmount = 'custom')}
							class:selected={selectedAmount === 'custom'}
						>
							CUSTOM
						</button>
					</div>
					<TextField label="Amount" name="amount" value={amount} />
					<TextField label="Name" name="name" value={name} />
					<TextField label="Email" name="email" value={email} />
					<button class="btn btn-secondary btn-block">DONATE</button>
				</form>
			</div>
		</div>

		<!-- carousel -->
		<div
			class="hero__background carousel-container"
			use:emblaCarouselSvelte={{ options, plugins }}
			onemblaInit={onInit}
		>
			<div class="carousel">
				{#each heroBgImages as coCurricularActivitiesImage}
					<img src={coCurricularActivitiesImage} alt="Bright Futures Foundation" class="item" />
				{/each}
			</div>
		</div>
	</section>

	<section class="about-us">
		<h2>About us</h2>
	</section>
</main>

<style lang="scss">
	@use '../styles/utils';
	.hero {
		--height: 70vh;
		position: relative;
		height: var(--height);
		width: 100vw;
		transition: opacity 0.5s ease-in-out;

		@include utils.respond-to('lg-screens') {
			--height: 72vh;
		}

		&::before {
			content: '';
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			background-image: var(--bg-image);
			background-size: cover;
			background-position: center;
			opacity: 1;
			transition: opacity 0.5s ease-in-out;
			z-index: -1; /* Keeps it behind the actual hero section content */
		}

		&::before {
			opacity: 1;
		}

		/* Trigger fade by toggling opacity */
		&::before {
			opacity: 0;
		}

		&__content {
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			position: absolute;
			height: 100%;
			width: 100%;
			color: #fff;
			z-index: utils.z('hero', 'content');
			display: flex;
			flex-direction: column;
			@include utils.add-section-lr-padding();
			@include utils.respond-to('lg-screens') {
				display: grid;
				grid-template-columns: 3fr 2fr;
				gap: var(--spacing-xl);
			}

			.copy {
				text-align: start;
				display: flex;
				align-items: start;
				justify-content: center;
				flex-direction: column;
				padding-top: var(--spacing-2xl);

				@include utils.respond-to('md-screens') {
					padding-top: var(--spacing-2xl);
				}

				.heading {
					font-size: var(--fs-md);
					line-height: 1.2;

					@include utils.respond-to('md-screens') {
						font-size: var(--fs-xl);
					}

					@include utils.respond-to('lg-screens') {
						font-size: var(--fs-2xl);
					}
				}

				.subheading {
					font-weight: var(--fw-medium);
					margin-top: var(--spacing-md);
					color: #e2e2e2ff;

					@include utils.respond-to('lg-screens') {
						width: 80%;
					}
				}

				.cta {
					display: flex;
					gap: var(--spacing-md);
				}
			}

			.form-container {
				display: flex;
				align-items: center;
				justify-content: center;
				position: absolute;
				bottom: -70%;
				width: 88%;
				@media screen and (min-height: 600px) {
					bottom: -54%;
				}
				@media screen and (min-height: 700px) {
					bottom: -32%;
				}
				@media screen and (min-height: 780px) {
					bottom: -39%;
				}
				@media screen and (min-height: 800px) {
					bottom: -24%;
				}
				@media screen and (min-height: 900px) {
					bottom: -14%;
				}
				@include utils.respond-to('lg-screens') {
					position: static;
				}

				.form {
					display: block;
					width: 100%;
					height: fit-content;
					padding: var(--spacing-xl);
					border-radius: var(--radius-xs);
					background-color: var(--clr-bg-primary);
					box-shadow: var(--shadow-md);

					h2 {
						text-align: center;
						color: var(--clr-txt-primary-on-bg-primary);
					}

					.btns {
						display: flex;
						align-items: center;
						justify-content: center;
						gap: var(--spacing-sm);

						button {
							padding: var(--spacing-sm);
							line-height: 1;
							border: 1px solid var(--clr-border-primary-on-bg-bg-primary);
							text-align: center;
							color: var(--clr-txt-primary-on-bg-primary);
							font-weight: var(--fw-semibold);

							&.selected {
								border: 1px solid var(--clr-accent-1);
								color: var(--clr-accent-1);
							}

							&:hover {
								border: 1px solid var(--clr-accent-1);
								color: var(--clr-accent-1);
							}
						}
					}
				}
			}
		}

		&__overlay {
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			height: 100%;
			width: 100%;
			background-color: #00000084;
			z-index: utils.z('hero', 'overlay');
		}

		&__background {
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			height: 100%;
			width: 100%;
			z-index: utils.z('hero', 'background');

			img {
				object-fit: cover;
			}
		}
	}

	.about-us {
		margin-top: 320px;

		@media screen and (min-height: 600px) {
			margin-top: 280px;
		}
		@media screen and (min-height: 700px) {
			margin-top: 240px;
		}
		@media screen and (min-height: 800px) {
			margin-top: 200px;
		}
		@media screen and (min-height: 900px) {
			margin-top: 180px;
		}

		@include utils.respond-to('lg-screens') {
			margin-top: 0px;
		}
	}
</style>
