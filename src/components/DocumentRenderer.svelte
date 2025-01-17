<script lang="ts">
  import type { TDocumentDefinitions } from 'pdfmake/interfaces'
  import ContentRenderer from './ContentRenderer.svelte'
  import {
    getPageStyleString,
    getPageContentStyleString,
  } from '../styling/page'
  import type { PageSizeMode } from '../styling/page'
  import HeaderFooterRenderer from './HeaderFooterRenderer.svelte'
  import { getFooterHeight, getHeaderHeight } from '../styling/header'
  import BackgroundRenderer from './BackgroundRenderer.svelte'
  import WatermarkRenderer from './WatermarkRenderer.svelte'

  export let document: TDocumentDefinitions
  export let mode: PageSizeMode

  let clientWidth: number
</script>

<div bind:clientWidth class="phr-container">
  <div
    class="phr-page"
    style={getPageStyleString(document, clientWidth, mode)}
  >
    {#if document.background}
      <div class="phr-backgroundLayer">
        <BackgroundRenderer node={document.background} />
      </div>
    {/if}
    <div class="phr-pageContent" style={getPageContentStyleString(document)}>
      {#if document.header}
        <div class="phr-header" style="height: {getHeaderHeight(document)}">
          <HeaderFooterRenderer node={document.header} />
        </div>
      {/if}
      {#if document.content}
        <ContentRenderer node={document.content} />
      {/if}
      {#if document.footer}
        <div class="phr-footer" style="height: {getFooterHeight(document)}">
          <HeaderFooterRenderer node={document.footer} />
        </div>
      {/if}
      {#if document.watermark}
        <WatermarkRenderer node={document.watermark} />
      {/if}
    </div>
  </div>
</div>

<style>
  .phr-container {
    max-width: 100%;
  }
  .phr-page {
    position: relative;
    display: flex;
    flex-direction: column;
    margin: auto;
    box-sizing: border-box;
    background: #fff;
    font-family: Roboto, Helvetica, sans-serif;
    font-size: 12pt;
    --font-size: 12pt;
    font-weight: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: normal;
    text-decoration: none;
    overflow: hidden;
  }
  .phr-header {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    overflow: hidden;
  }
  .phr-pageContent {
    position: relative;
    flex: 1 1 auto;
    z-index: 1;
  }
  .phr-footer {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    overflow: hidden;
  }
  .phr-backgroundLayer {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
  }
</style>
