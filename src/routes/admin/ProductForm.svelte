<script>
    import { createClient } from '@supabase/supabase-js';
    import { Editor, Viewer } from 'bytemd'
    import gfm from '@bytemd/plugin-gfm'
    import breaks from '@bytemd/plugin-breaks'

    const plugins = [
      gfm(),
      breaks()
    ]

    // Initialize Supabase client
    // const supabaseUrl = 'YOUR_SUPABASE_URL';
    // const supabaseKey = 'YOUR_SUPABASE_ANON_KEY';
    // const supabase = createClient(supabaseUrl, supabaseKey);
  
    let name = '';
    let description = '';
    let price = '';
    let startDate = '';
    let endDate = '';
    let imageUrl = '';
  
    function handleChange(e) {
      description = e.detail.value
    }

    const submitForm = async () => {
  
      // const { data, error } = await supabase
      //   .from('your_table_name')
      //   .insert([
      //     {
      //       name,
      //       description,
      //       price: parseFloat(price),
      //       start_date: startDate,
      //       end_date: endDate,
      //       image_url: imageUrl,
      //     },
      //   ]);
  
      // if (error) {
      //   console.error('Error:', error);
      // } else {
      //   console.log('Success:', data);
      // }
    };
  </script>
  
  <form on:submit|preventDefault={submitForm} class="flex flex-col max-w-xl mx-auto p-4">
    <label for="name" class="mb-2 text-lg font-medium">Name</label>
    <input type="text" id="name" bind:value={name} required class="mb-4 p-2 border border-gray-300 rounded" />
  
    <label for="description" class="mb-2 text-lg font-medium">Description</label>
    <Editor mode="split" value={description} {plugins} on:change={handleChange} />
  
    <label for="price" class="mb-2 text-lg font-medium">Price</label>
    <input type="number" id="price" bind:value={price} step="0.01" required class="mb-4 p-2 border border-gray-300 rounded" />
  
    <label for="startDate" class="mb-2 text-lg font-medium">Start Date</label>
    <input type="date" id="startDate" bind:value={startDate} required class="mb-4 p-2 border border-gray-300 rounded" />
  
    <label for="endDate" class="mb-2 text-lg font-medium">End Date</label>
    <input type="date" id="endDate" bind:value={endDate} required class="mb-4 p-2 border border-gray-300 rounded" />
  
    <label for="imageUrl" class="mb-2 text-lg font-medium">Image URL</label>
    <input type="text" id="imageUrl" bind:value={imageUrl} class="mb-4 p-2 border border-gray-300 rounded" />
  
    <button type="submit" class="p-2 bg-blue-500 text-white rounded hover:bg-blue-600">Submit</button>
  </form>
  