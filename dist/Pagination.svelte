<script>import { createEventDispatcher } from "svelte";
const dispatch = createEventDispatcher();
export let propActivePage = 1;
export let propNumberOfPages = 5;
export let propPrevious = "PREVIOUS";
export let propNext = "NEXT";
export let propSize = "default";
export let cssActiveBackgroundColor = "#0d6efd";
export let cssBackgroundColor = "#fff";
export let cssActiveBorderColor = "#0d6efd";
export let cssActiveFontColor = "white";
export let cssHoverBackgroundColor = "#f8f9fa";
export let cssFontColor = "#0d6efd";
export let cssHoverFontColor = "#0a58ca";
export let cssBorderWidth = "1px";
export let cssDisabledColor = "rgba(33, 37, 41, 0.75)";
export let cssBorderColor = "#dee2e6";
export let maxPageDisplay = null;
let shortify = [];
function pagination() {
  if (!maxPageDisplay) {
    return [];
  }
  const m = maxPageDisplay / 2;
  var left = 0, right = 0, range = [], rangeWithDots = [], l;
  if (propActivePage === 1) {
    right = maxPageDisplay;
  } else {
    if (m % 2 === 0) {
      left = propActivePage - m;
      right = propActivePage + m + 1;
    } else {
      left = propActivePage - m;
      right = propActivePage + m;
    }
  }
  for (let i = 1; i <= propNumberOfPages; i++) {
    if (i == 1 || i == propNumberOfPages || i >= left && i < right) {
      range.push(i);
    }
  }
  for (let i of range) {
    if (l) {
      if (i - l === 2) {
        rangeWithDots.push(l + 1);
      } else if (i - l !== 1) {
        rangeWithDots.push(-1);
      }
    }
    rangeWithDots.push(i);
    l = i;
  }
  return rangeWithDots;
}
$:
  propActivePage && (() => {
    if (maxPageDisplay && maxPageDisplay < propNumberOfPages) {
      shortify = pagination();
    }
  })();
</script>

<div
	class="main"
	style="
		display:flex;
		--cssActiveBackgroundColor: {cssActiveBackgroundColor};
		--cssActiveFontColor:{cssActiveFontColor};
		--cssActiveBorderColor:{cssActiveBorderColor};
		--cssHoverBackgroundColor:{cssHoverBackgroundColor};
		--cssFontColor:{cssFontColor};
		--cssBackgroundColor:{cssBackgroundColor};
		--cssHoverFontColor:{cssHoverFontColor};
		--cssBorderWidth:{cssBorderWidth};
		--cssDisabledColor:{cssDisabledColor};
		--cssBorderColor:{cssBorderColor};
	"
>
	<div on:click on:keydown role="button" tabindex="0">
		<nav style="overflow-x: auto; -webkit-overflow-scrolling: touch;">
			<ul
				class="pagination"
				class:pagination-sm={propSize === 'small'}
				class:pagination-lg={propSize === 'large'}
			>
				{#if propActivePage === 1}
					<li class="page-item disabled" style="cursor:not-allowed">
						<span class="page-link">{propPrevious}</span>
					</li>
				{:else}
					<li class="page-item" style="cursor: pointer">
						<div
							on:keydown
							role="button"
							tabindex="0"
							on:click={() => {
								propActivePage = propActivePage - 1;
								dispatch('eventChange', {
									numberActivePage: propActivePage
								});
							}}
						>
							<span class="page-link">{propPrevious}</span>
						</div>
					</li>
				{/if}

				{#if shortify.length}
					{#each shortify as item}
						{#if item === -1}
							<span class="page-link">...</span>
						{:else}
							<li
								class="page-item"
								class:active={item === propActivePage}
								style:cursor={propActivePage === item ? 'default' : 'pointer'}
							>
								<div
									on:keydown
									role="button"
									tabindex="0"
									on:click={() => {
										propActivePage = item;
										dispatch('eventChange', {
											numberActivePage: propActivePage
										});
									}}
								>
									<span class="page-link">{item}</span>
								</div>
							</li>
						{/if}
					{/each}
				{:else}
					{#each Array(propNumberOfPages) as _, numberCounter}
						<li
							class="page-item"
							class:active={numberCounter + 1 === propActivePage}
							style:cursor={propActivePage === numberCounter + 1 ? 'default' : 'pointer'}
						>
							<div
								on:keydown
								role="button"
								tabindex="0"
								on:click={() => {
									propActivePage = numberCounter + 1;
									dispatch('eventChange', {
										numberActivePage: propActivePage
									});
								}}
							>
								<span class="page-link">{numberCounter + 1}</span>
							</div>
						</li>
					{/each}
				{/if}
				{#if propActivePage === propNumberOfPages}
					<li class="page-item disabled" style="cursor:not-allowed;">
						<span class="page-link">{propNext}</span>
					</li>
				{:else}
					<li class="page-item" style="cursor:pointer">
						<div
							on:keydown
							role="button"
							tabindex="0"
							on:click={() => {
								propActivePage = propActivePage + 1;
								dispatch('eventChange', {
									numberActivePage: propActivePage
								});
							}}
						>
							<span class="page-link">{propNext}</span>
						</div>
					</li>
				{/if}
			</ul>
		</nav>
	</div>
</div>

<style>
	.main {
		--bs-font-sans-serif: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue',
			'Noto Sans', 'Liberation Sans', Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
			'Segoe UI Symbol', 'Noto Color Emoji';
		--bs-body-font-family: var(--bs-font-sans-serif);
		--bs-secondary-bg: #e9ecef;
		font-family: var(--bs-body-font-family);
		line-height: 1.5;
	}
	ul {
		margin-top: 0;
		margin-bottom: 0;
	}
	.pagination {
		--bs-pagination-padding-x: 0.75rem;
		--bs-pagination-padding-y: 0.375rem;
		--bs-pagination-font-size: 1rem;
		--bs-pagination-color: var(--cssFontColor);
		--bs-pagination-bg: var(--cssBackgroundColor);
		--bs-pagination-border-width: var(--cssBorderWidth);
		--bs-pagination-border-color: var(--cssBorderColor);
		--bs-pagination-border-radius: 0.375rem;
		--bs-pagination-hover-color: var(--cssHoverFontColor);
		--bs-pagination-hover-bg: var(--cssHoverBackgroundColor);
		--bs-pagination-hover-border-color: var(--cssBorderColor);
		--bs-pagination-focus-color: var(--cssHoverFontColor);
		--bs-pagination-focus-bg: var(--bs-secondary-bg);
		--bs-pagination-focus-box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.25);
		--bs-pagination-active-color: var(--cssActiveFontColor);
		--bs-pagination-active-bg: var(--cssActiveBackgroundColor);
		--bs-pagination-active-border-color: var(--cssActiveBorderColor);
		--bs-pagination-disabled-bg: var(--bs-secondary-bg);
		--bs-pagination-disabled-border-color: var(--cssBorderColor);
		display: flex;
		padding-left: 0;
		list-style: none;
	}
	.page-link {
		position: relative;
		display: block;
		padding: var(--bs-pagination-padding-y) var(--bs-pagination-padding-x);
		font-size: var(--bs-pagination-font-size);
		color: var(--bs-pagination-color);
		text-decoration: none;
		background-color: var(--bs-pagination-bg);
		border: var(--bs-pagination-border-width) solid var(--bs-pagination-border-color);
		transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
			border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
	}
	@media (prefers-reduced-motion: reduce) {
		.page-link {
			transition: none;
		}
	}
	.page-link:hover {
		z-index: 2;
		color: var(--bs-pagination-hover-color);
		background-color: var(--bs-pagination-hover-bg);
		border-color: var(--bs-pagination-hover-border-color);
	}
	.page-link:focus {
		z-index: 3;
		color: var(--bs-pagination-focus-color);
		background-color: var(--bs-pagination-focus-bg);
		outline: 0;
		box-shadow: var(--bs-pagination-focus-box-shadow);
	}
	.active .page-link {
		z-index: 3;
		color: var(--bs-pagination-active-color);
		background-color: var(--bs-pagination-active-bg);
		border-color: var(--bs-pagination-active-border-color);
	}
	.disabled > .page-link {
		color: var(--cssDisabledColor);
		pointer-events: none;
		background-color: var(--bs-pagination-disabled-bg);
		border-color: var(--bs-pagination-disabled-border-color);
	}
	.page-item:not(:first-child) .page-link {
		margin-left: calc(var(--cssBorderWidth) * -1);
	}
	.page-item:first-child .page-link {
		border-top-left-radius: var(--bs-pagination-border-radius);
		border-bottom-left-radius: var(--bs-pagination-border-radius);
	}
	.page-item:last-child .page-link {
		border-top-right-radius: var(--bs-pagination-border-radius);
		border-bottom-right-radius: var(--bs-pagination-border-radius);
	}
	.pagination-lg {
		--bs-pagination-padding-x: 1.5rem;
		--bs-pagination-padding-y: 0.75rem;
		--bs-pagination-font-size: 1.25rem;
		--bs-pagination-border-radius: 0.5rem;
	}
	.pagination-sm {
		--bs-pagination-padding-x: 0.5rem;
		--bs-pagination-padding-y: 0.25rem;
		--bs-pagination-font-size: 0.875rem;
		--bs-pagination-border-radius: 0.25rem;
	}
</style>
