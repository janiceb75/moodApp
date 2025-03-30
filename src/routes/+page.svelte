<h1>My Mood</h1>
<p>How Are You Feeling Today?</p>
<br>
<select style="font-size:28px" bind:value={moodInput}>
    <option>😀 Happy</option>
    <option>😢 Sad</option>
    <option>😠 Angry</option>
    <option>😰 Anxious</option>
    <option>😒 Jealous</option>
    <option>😕 Confused</option>
    <option>🥺 Lonely</option>
    <option>🙏 Grateful</option>
    <option>😄 Excited</option>
    <option>😎 Confident</option>
</select>
<br><br>
<textarea  rows="5" cols="30" style="width:40%; height:100px; resize:both;" bind:value={moodText}>
</textarea>
<br><br><br>

<button on:click={moodFunc}>Enter in Journal</button>

<button on:click={readJournal}>Read Journal</button>
<br>
<!--{journalMessages}<!--cant do this cause it tries to convert it to a string cause we in HTML-->
{#if journalMessages.length>0}
    <ul>
        {#each journalMessages as entry}
            <li>
                <strong>Mood:</strong>{entry.mood}<br>
                <strong>Entry:</strong>{entry.journal_entry}
            </li>
        {/each}
    </ul>
{:else}
    <p>No entries found.</p>
{/if}

<script>

    
    let moodInput;
    let moodText;
    let journalMessages=[];

    import {createClient} from "@supabase/supabase-js"
    const supabase=createClient('https://ggwbvoxyjczfbhsiyuaz.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Imdnd2J2b3h5amN6ZmJoc2l5dWF6Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDA5MjQyODAsImV4cCI6MjA1NjUwMDI4MH0.VhkEhjj9iBQux-hyeg44562tZrFHAKE-tHLQy4w5Koo')

    async function moodFunc(){
        

    const {data,error}= await supabase
    .from("mood")
    .insert({journal_entry:moodText, mood:moodInput})
    

    if (error) {
    console.error("❌ Insert error:", error);
  } else {
    console.log("✅ Mood entry added:", data);
  }
    
    }

    async function readJournal(){
        const {data, error} = await supabase
        .from("mood")
        .select("*")
    

    if(error){
        console.error("Error getting entries from mood table");
    }else{
        console.log("Moods retrieved", data);
        journalMessages=data;
    }
    }
</script>