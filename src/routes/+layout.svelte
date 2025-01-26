<script lang="ts">
	import '../styles/global.scss';

	import { navigating } from '$app/state';
	import { afterNavigate } from '$app/navigation';

	import NavLink from './NavLink.svelte';

	const { children } = $props();

	let openNav = $state(false);

	afterNavigate(() => {
		openNav = false;
	});
</script>

<svelte:head>
	<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
	<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
	<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
	<link rel="manifest" href="/site.webmanifest" />
	<meta name="theme-color" content="#16181d" />
	<link rel="stylesheet" href="/fonts/remixicon/remixicon.css" />
	<!-- <link rel="manifest" href="manifest.webmanifest" /> -->
	<link rel="alternate" href="/rss.xml" type="application/rss+xml" title="RSS" />
</svelte:head>

{#if navigating}
	<div class="loader"></div>
{/if}

<header class="header">
	<a href="/" class="header__logo">
		<img src="/images/header-logo.png" alt="Bright Futures Foundation logo" />
	</a>

	<nav class="header__nav" class:open={openNav}>
		<NavLink href="/" name="Home" />
		<NavLink href="/about-us" name="About us" />
		<NavLink href="/contact-us" name="Contact us" />
		<a href="/donate" class="mobile-cta btn btn-secondary">Donate</a>
	</nav>

	<a href="/donate" class="header__desktop-cta btn btn-secondary">Donate</a>

	<button class="header__burger" aria-label="Open menu" onclick={() => (openNav = !openNav)}>
		{#if openNav}
			<i class="ri-close-fill"></i>
		{:else}
			<i class="ri-menu-line"></i>
		{/if}
	</button>
</header>

<div class="children">
	{@render children()}
</div>

<footer class="footer">
	<div class="footer__top-section">
		<div class="about-us">
			<img
				src="/images/footer-logo.png"
				alt="Bright Futures Foundation logo"
				class="about-us__logo"
			/>
			<p class="about-us__copy">
				We are a non-profit organization dedicated to empowering communities in Uganda through
				education, healthcare, and economic development.
			</p>
		</div>

		<nav class="nav pages">
			<h4>Pages</h4>
			<a href="/">Home</a>
			<a href="/about-us">About us</a>
			<a href="/contact-us">Contact us</a>
		</nav>

		<nav class="nav contact-us">
			<h4>Contact us</h4>

			<a href="tel:+256700000000" target="_blank">
				<span>
					<i class="ri-phone-line"></i>
				</span>
				<span>+256700000000</span>
			</a>
			<a href="https://wa.me/256700000000" target="_blank">
				<span>
					<i class="ri-whatsapp-line"></i>
				</span>
				<span>+256700000000</span>
			</a>
			<a href="mailto:info@brightfuturesfoundation.com" target="_blank">
				<span>
					<i class="ri-mail-line"></i>
				</span>
				<span>info@brightfuturesfoundation.com</span>
			</a>
		</nav>

		<nav class="nav follow-us">
			<h4>Follow us</h4>

			<div>
				<a href="https://www.x.com/" aria-label="Follow us on x">
					<i class="ri-twitter-x-fill"></i>
				</a>

				<a href="https://www.facebook.com/" aria-label="Follow us on Facebook">
					<i class="ri-facebook-circle-fill"></i>
				</a>

				<a href="https://www.instagram.com/" aria-label="Follow us on Instagram">
					<i class="ri-instagram-fill"></i>
				</a>
			</div>
		</nav>
	</div>

	<div class="footer__bottom-section">
		<p class="copyright">
			&copy; Bright Futures Foundation {new Date().getFullYear()}. All rights reserved.
		</p>
	</div>
</footer>

<style lang="scss">
	@use '../styles/utils';

	.header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: utils.z('header', 'nav');
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-top: var(--spacing-sm);
		padding-bottom: var(--spacing-sm);
		background-color: var(--clr-bg-primary);
		color: var(--clr-txt-primary-on-bg-primary);
		height: var(--height-header);
		border-bottom: 1px solid var(--clr-border-primary-on-bg-bg-primary);
		box-shadow: var(--shadow-sm);
		@include utils.add-section-lr-padding();

		&__logo {
			height: 86%;
			text-decoration: none;

			img {
				height: 100%;
			}
		}

		&__nav {
			display: flex;
			gap: var(--spacing-lg);
			position: absolute;
			top: var(--header-height);
			left: 0;
			right: 0;
			z-index: utils.z('header', 'nav');
			visibility: hidden;
			opacity: 0;
			transform: translate(10px, -10px);
			pointer-events: none;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: flex-start;
			background-color: inherit;
			color: inherit;
			width: 100vw;
			height: fit-content;
			overflow: hidden;
			padding: {
				top: var(--spacing-md);
				bottom: var(--spacing-md);
			}
			border-bottom: 1px solid var(--clr-border-primary-on-bg-bg-primary);
			box-shadow: var(--shadow-sm);
			@include utils.add-section-lr-padding();

			@include utils.respond-to('lg-screens') {
				position: static;
				flex-direction: row;
				visibility: visible;
				transform: none;
				pointer-events: all;
				opacity: 1;
				width: fit-content;
				height: fit-content;
				border: none;
				gap: var(--spacing-xl);
				padding: 0;
				border-bottom: 0;
				box-shadow: none;
			}

			&.open {
				visibility: visible;
				opacity: 1;
				pointer-events: all;
				transform: none;
				transition: all 0.4s cubic-bezier(0.085, 1.735, 0.285, 0.995);
			}

			.mobile-cta {
				display: block;

				@include utils.respond-to('lg-screens') {
					display: none;
				}
			}
		}

		&__desktop-cta {
			display: none;

			@include utils.respond-to('lg-screens') {
				display: block;
			}
		}

		&__burger {
			line-height: 1;
			font-size: var(--fs-lg);
			padding: var(--spacing-md);
			display: block;

			@include utils.respond-to('lg-screens') {
				display: none;
			}
		}
	}

	.children {
		margin-top: var(--header-height);
		min-height: calc(80vh - var(--header-height));
	}

	.footer {
		background-color: var(--clr-bg-accent-1);
		color: var(--clr-txt-primary-on-bg-accent-1);
		text-align: center;
		display: flex;
		flex-direction: column;
		gap: var(--spacing-xs);
		margin: var(--spacing-lg) 0 0 0;
		padding-top: var(--spacing-lg);
		padding-bottom: var(--spacing-lg);
		@include utils.add-section-lr-padding();

		&__top-section {
			display: flex;
			flex-direction: column;
			align-items: flex-start;
			color: var(--clr-txt-primary-on-bg-accent-1);
			gap: var(--spacing-lg);

			@include utils.respond-to('lg-screens') {
				display: grid;
				grid-template-columns: 1fr 1fr 1fr 1fr;
				gap: var(--spacing-2xl);
			}

			.nav {
				display: flex;
				flex-direction: column;
				align-items: flex-start;
				color: var(--clr-txt-primary-on-bg-accent-1);

				h4 {
					font-weight: var(--fw-bold);
					text-transform: uppercase;
					color: var(--clr-txt-primary-on-bg-accent-1);
				}

				a {
					text-decoration: none;
					font-size: var(--fs-sm);
					color: var(--clr-txt-primary-on-bg-accent-1);
				}
			}

			.about-us {
				display: flex;
				flex-direction: column;
				align-items: flex-start;
				gap: var(--spacing-sm);

				&__logo {
					height: 56px;
				}

				&__copy {
					text-align: left;
					font-size: var(--fs-sm);
				}
			}

			.contact-us {
				a {
					display: flex;
					align-items: center;
					gap: var(--spacing-xs);
					color: var(--clr-txt-primary-on-bg-accent-1);

					span:nth-of-type(1) {
						font-size: var(--fs-md);
					}
				}
			}

			.follow-us {
				div {
					display: flex;
					gap: var(--spacing-md);

					a {
						background-color: var(--clr-bg-primary);
						color: var(--clr-accent-1);
						padding: var(--spacing-sm);
						font-size: var(--fs-md);
						text-decoration: none;
						border-radius: 50%;
						line-height: 1;
					}
				}
			}
		}

		&__bottom-section {
			margin-top: var(--spacing-md);
			padding-top: var(--spacing-md);
			border-top: 1px solid var(--clr-border-secondary-on-bg-accent-1);
			.copyright {
				font-size: var(--fs-sm);
			}
		}
	}
</style>
