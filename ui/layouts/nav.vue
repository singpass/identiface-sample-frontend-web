<template>
    <b-navbar>
        <template slot="start">
            <b-navbar-item tag="router-link" :to="{path: '/'}">
                Home
            </b-navbar-item>
        </template>
        <template slot="end">
            <b-navbar-item tag="router-link" :to="{path: '/docs'}">
                QuickStart Docs
            </b-navbar-item>
            <b-navbar-item tag="router-link" :to="{path: '/html_sdk'}">
                Sample Registration Flow <b-tag :rounded="true" type="is-pill" style="margin-left:0.5rem;">Face Verify</b-tag>
            </b-navbar-item>
            <!-- <b-navbar-item tag="router-link" :to="{path: '/compare'}">
                Upload user image <b-tag :rounded="true" type="is-pill" style="margin-left:0.5rem;">Face Compare Sample</b-tag>
            </b-navbar-item> -->
        </template>
    </b-navbar>
</template>