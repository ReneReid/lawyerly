<template>
  <nav
    class="navbar is-fixed-top"
    role="navigation"
    aria-label="main navigation"
  >
    <div class="navbar-brand">
      <a class="navbar-item" href="https://github.com/LawyerlyOrg/lawyerly">
        <img src="@/assets/lawyerly_logo.png" width="112" height="28" />
      </a>
    </div>
  </nav>

  <div id="app">
    <div class="container">
      <h6
        class="subtitle is-6"
        style="text-align: left; margin-bottom: 0; margin-left: 10px"
      >
        <strong>Lawyerly</strong> is a GPT-powered research tool designed to
        save lawyers time by distilling mountains of legal documents into
        information relevant to their case!
      </h6>

      <h6
        class="subtitle is-6"
        style="
          text-align: left;
          margin-left: 10px;
          margin-top: 5px;
          color: grey;
        "
      >
        <!-- <em
          >Created using: Flask (Python), Vue.js, MongoDB, OpenAI API,
          Langchain, Pinecone</em -->
        <!-- > -->
      </h6>

      <Wizard
        squared-tabs
        card-background
        navigable-tabs
        scrollable-tabs
        :nextButton="nextButtonOptions"
        :custom-tabs="[
          {
            title: 'Select Fact Sheet',
          },
          {
            title: 'Documents',
          },
          {
            title: 'Report',
          },
        ]"
        :beforeChange="onTabBeforeChange"
        @change="onChangeCurrentTab"
        @complete:wizard="wizardCompleted"
      >
        <h5 v-if="currentTabIndex === 0">
          <div>
            <SelectFactSheet
              v-bind:child-prop="selectedItem"
              v-on:custom-event="updateSelectedItem"
            />
          </div>
        </h5>
        <h5 v-if="currentTabIndex === 1">
          <SelectCases
            v-bind:child-prop="cases"
            v-on:custom-event="updateCases"
          />
        </h5>
        <h5 v-if="currentTabIndex === 2">
          <RelevanciesReport
            :selected-fact-sheet="selectedItem"
            :sharedCases="cases"
          />
        </h5>
      </Wizard>
    </div>

    <div class="container">
      <div class="card">
        <div class="card-content">
          <div class="content">
            <H1 class="title"> How We Made Lawyerly </H1>
            <p class="subtitle is-6">
              <em
                >Created using: Flask (Python), Vue.js, MongoDB, Google Cloud
                Platform (GCP), Vercel, OpenAI</em
              >
            </p>

            <iframe
              style="border: none"
              width="100%"
              height="450"
              src="https://whimsical.com/embed/VbkHMwvj38xjwBk6Ked6w1@2Ux7TurymN7h1RReB9y2"
            ></iframe>

            
            <h2>How We Ingest Documents</h2>
            <div class="content">
              <ol type="1" start="0">
                <li>User uploads their legal document PDF files.</li>
                <li>We extract plaintext out of the PDF and
              "chunk" it using <span class="has-text-weight-medium">Langchain</span>. Chunking splits text into blocks while
              maintaing all semantically relevant paragraphs together for as
              long as possible.</li>
                <li> We create embedding using the <span class="has-text-weight-medium">OpenAI API</span>,
              which is the process of representing text in a high-dimensional
              numerical vector space.</li>
                <li>We store the embedding in <span class="has-text-weight-medium">Pinecone</span>, a
              vector which indexes and stores vector embeddings for fast
              retrieval and similarity search</li>
              <li>We summarize each document by
              querying <span class="has-text-weight-medium">GPT-4</span> against the embeddings that we just stored in the
              previous step.</li>
              <li>We store each summary in <span class="has-text-weight-medium">MongoDB</span>!</li>
              </ol>
            </div>
            <h2>How We Create Reports</h2>
            <p>
            <ol type="1" start="0">
              <li>User uploads a fact sheet that they've created. A fact sheet
              is a document that includes the relevant details of a client's
              legal issue.</li>
              
              <li>User selects the legal documents they want to
              compare to fact sheet. Users can either select documents that have
              already been ingested (see above), or submit new ones to ingest.</li>

              <li>Lawyerly queries GPT-4 with the factsheet and the selected
              documents for comparison and evaluation of relevancy.</li>
              
              <li>The user receives a report of how each document is relevant to their fact
              sheet!</li>
              
              </ol>
            </p>
            <h2>Future Work</h2>
            <p>
              <ol type="1" start = "0">
                <li>Refactor Ingest such that it does not do batches of documents,
              but rather single document, and can be run in parallel for loads
              involving multiple documents.</li>

              <li> While MongoDB allowed
              flexibility in prototyping and deploying our product; our data
              models ended up requiring relationships that would have been much
              easier to implement with a relational database, such as PostGres.</li>

              <li>Chat function: allow users to directly chat with their
              documents.</li>

              </ol>
            </p>
            <!-- <h2>Tech Stack</h2> -->
            <!-- <div class="columns">
              <div class="column">
               
                <article class="message">
                  <div class="message-header">Hello World</div>
                  <div class="message-body">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    <strong>Pellentesque risus mi</strong>, tempus quis placerat
                    ut, porta nec nulla...
                   
                  </div>
                </article>
              </div>
              <div class="column">
               
                <article class="message">
                  <div class="message-header">Hello World</div>
                  <div class="message-body">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    <strong>Pellentesque risus mi</strong>, tempus quis placerat
                    ut, porta nec nulla...
                   
                  </div>
                </article>
              </div>
            </div>
            <div class="columns">
              <div class="column">
            
                <article class="message">
                  <div class="message-header">Hello World</div>
                  <div class="message-body">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    <strong>Pellentesque risus mi</strong>, tempus quis placerat
                    ut, porta nec nulla...
                   
                  </div>
                </article>
              </div>
              <div class="column">
            
                <article class="message">
                  <div class="message-header">Hello World</div>
                  <div class="message-body">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    <strong>Pellentesque risus mi</strong>, tempus quis placerat
                    ut, porta nec nulla...
                  
                  </div>
                </article>
              </div>
            </div> -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "form-wizard-vue3/dist/form-wizard-vue3.css";
import Wizard from "form-wizard-vue3";
import SelectFactSheet from "./components/SelectFactSheet.vue";
import SelectCases from "./components/SelectCases.vue";
import RelevanciesReport from "./components/RelevanciesReport.vue";

export default {
  name: "App",
  components: {
    Wizard,
    SelectFactSheet,
    SelectCases,
    RelevanciesReport,
  },
  data() {
    return {
      currentTabIndex: 0,
      selectedItem: "",
      cases: [],
      selectedFactSheet: false,
    };
  },
  methods: {
    updateSelectedItem(newValue) {
      this.selectedItem = newValue;
      this.selectedFactSheet = true;
    },
    updateCases(newCases) {
      this.cases = newCases;
    },
    onChangeCurrentTab(index, oldIndex) {
      console.log(index, oldIndex);
      this.currentTabIndex = index;
    },
    onTabBeforeChange() {
      if (this.currentTabIndex === 0) {
        console.log("First Tab");
      }
      console.log("All Tabs");
    },
    wizardCompleted() {
      console.log("Wizard Completed");
    },
  },
  computed: {
    nextButtonOptions() {
      return this.currentTabIndex === 2
        ? {
            text: "test",
            icon: "check",
            hideIcon: true, // default false but selected for sample
            hideText: false, // default false but selected for sample
            disabled: true,
          }
        : { disabled: !this.selectedFactSheet };
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5rem;
}

.container {
  max-width: 800px; /* or whatever width you prefer */
}

.card-content .content {
  text-align: left;
}

.message {
  max-width: 90%; /* Adjust this value to suit your design needs */
  margin-left: auto; /* Centering the element */
  margin-right: auto; /* Centering the element */
}

.message-header {
  margin: 0rem !important;
}
</style>
