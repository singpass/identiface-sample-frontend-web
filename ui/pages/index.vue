<template>
    <div class="container">
        <div class="section">
            <div class="columns">
                <div class="column has-text-centered">
                    <h1 class="title">Identiface Sample Reference Journeys</h1><br>
                </div>
            </div>
            <div class="row columns is-multiline">
                <div class="column is-4">
                    <div class="card large">
                        <div class="card-content">
                            <p class="title">
                            User Registration
                            </p>
                            <p class="subtitle">
                            with Identiface (button validation)
                            </p>
                            <b-tag :rounded="true" type="is-pill" style="padding: 1rem !important;"><b>Face Verify</b></b-tag>
                        </div>
                        <footer class="card-footer">
                            <p class="card-footer-item">
                            <span>
                                View it <a href="/sample-app/registrations">here</a>
                            </span>
                            </p>
                            <p class="card-footer-item">
                            <span>
                                <!-- See on <a href="#">GitHub</a> -->
                            </span>
                            </p>
                        </footer>
                    </div>
                </div>
                <div class="column is-4">
                    <div class="card large">
                        <div class="card-content">
                            <p class="title">
                            Transactions
                            </p>
                            <p class="subtitle">
                            with Identiface (event-driven validation)
                            </p>
                            <b-tag :rounded="true" type="is-pill" style="padding: 1rem !important;"><b>Face Verify</b></b-tag>
                        </div>
                        <footer class="card-footer">
                            <p class="card-footer-item">
                            <span>
                                View it <a href="/sample-app/transactions">here</a>
                            </span>
                            </p>
                            <p class="card-footer-item">
                            <span>
                                <!-- See on <a href="#">GitHub</a> -->
                            </span>
                            </p>
                        </footer>
                    </div>
                </div>
                <div class="column is-4">
                    <div class="card large">
                        <div class="card-content">
                            <p class="title">
                            New Employee Image
                            </p>
                            <p class="subtitle">
                            with Identiface
                            </p>
                            <b-tag :rounded="true" type="is-pill-blue" style="padding: 1rem !important;"><b>Face Compare</b></b-tag>
                        </div>
                        <footer class="card-footer">
                            <p class="card-footer-item">
                            <span>
                                View it <a href="/sample-app/employee-image">here</a>
                            </span>
                            </p>
                            <p class="card-footer-item">
                            <span>
                                <!-- See on <a href="#">GitHub</a> -->
                            </span>
                            </p>
                        </footer>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    // add head to include the generated client Javascript file => Script Tag
    head () {
        return {
            title: 'Identiface | Sample User Flows',
        }
    },
}
</script>