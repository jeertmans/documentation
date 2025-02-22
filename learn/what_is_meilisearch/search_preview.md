# Search preview

After adding documents to your Meilisearch instance, you can immediately start searching through your dataset using Meilisearch's search preview. This can be useful when you want to check that documents have been properly indexed and Meilisearch is working as expected.

If your Meilisearch instance does not have any indexes, you should see this screen.

![Meilisearch search preview instructing the user to set an API key and configure an index](/search_preview/no_documents.png)

To access the search preview in your browser, navigate to the address and port specified in the command line argument `--http-addr`. If you did not configure `--http-addr` when launching your instance, [Meilisearch's default address for the search preview is 127.0.0.1:7700](/learn/configuration/instance_options.md#http-address-port-binding).

For security reasons, the search preview is only available in [development mode](/learn/configuration/instance_options.md#environment). You can disable it by setting the `MEILI_ENV` environment variable or `--env` CLI option to `production`. Note that you must set a [master key](/learn/configuration/instance_options.md#master-key) in production mode.

### Example

You can add a dataset containing many popular movies to an instance:

<CodeSamples id="add_movies_json_1" />

Once indexing finishes, you can access `http://127.0.0.1:7700` and confirm everything is working correctly by searching for "The Truman Show:"

<MovieGif />
