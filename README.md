# select22
Select2 vue wrapper

            <select22
              name="clients_list"
              :options="sortedClients"
              :dynamic_options="searchresult"
              v-model="form1.client_id"
              :disabled="isDisabled"
              swidth="90%"
              @search="searchClient"
            />
           
