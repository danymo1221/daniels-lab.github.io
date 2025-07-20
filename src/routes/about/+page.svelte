<script lang="ts">
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import * as Card from "$lib/components/ui/card/index.js";
    import Button from "$lib/components/ui/button/button.svelte";
    import * as Accordion from "$lib/components/ui/accordion/index.js";
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import { pageTitle } from "$lib/stores/title";
    import { onMount } from "svelte";
    pageTitle.set("About me");

    let setupData: Array<{ type: string; device: string; specs: string }> = [];
    let loading = true;
    let error = "";
    onMount(async () => {
      try {
        const res = await fetch("/data/desktop_setup.json");
        if (!res.ok) throw new Error("Failed to fetch setup data");
        setupData = await res.json();
      } catch (e) {
        error = (e instanceof Error ? e.message : String(e)) || "Unknown error";
      } finally {
        loading = false;
      }
    });
</script>

<div class="flex flex-col justify-center items-center min-h-screen px-4 mb-5">
  <div class="flex justify-center w-full max-w-screen-xl mx-auto mt-5">
    <Card.Root class="bg-white/10 backdrop-blur-2xl border border-white/30 rounded-xl text-white shadow-lg p-6 w-full max-w-[540px]">
      <Card.Content>
        <Tabs.Root value="about">
          <div class="flex justify-center w-full">
            <Tabs.List class="inline-flex gap-3 mb-6 bg-white/10 backdrop-blur rounded-xl p-3">
              <Tabs.Trigger value="about" class="px-4 py-2 rounded-lg font-semibold transition-all data-[state=active]:bg-white/20 data-[state=active]:border-b-4 data-[state=active]:border-indigo-300 data-[state=active]:text-indigo-100 data-[state=inactive]:text-neutral-300 data-[state=inactive]:hover:bg-white/10">About</Tabs.Trigger>
              <Tabs.Trigger value="skills" class="px-4 py-2 rounded-lg font-semibold transition-all data-[state=active]:bg-white/20 data-[state=active]:border-b-4 data-[state=active]:border-indigo-300 data-[state=active]:text-indigo-100 data-[state=inactive]:text-neutral-300 data-[state=inactive]:hover:bg-white/10">Skills & Certifications</Tabs.Trigger>
              <Tabs.Trigger value="setup" class="px-4 py-2 rounded-lg font-semibold transition-all data-[state=active]:bg-white/20 data-[state=active]:border-b-4 data-[state=active]:border-indigo-300 data-[state=active]:text-indigo-100 data-[state=inactive]:text-neutral-300 data-[state=inactive]:hover:bg-white/10">Setup</Tabs.Trigger>
            </Tabs.List>
          </div>

          <Tabs.Content value="about">
            <div class="space-y-4">
              <Card.Title class="text-center">About Me</Card.Title>
              <p>Hey! I’m Daniel — a 24-year-old tinkerer with a passion for law and IT.</p>
              <p>I’m currently working as a Project & Growth Manager at ZAP-Hosting, while also diving into the technical side as a System Integration Specialist.</p>
              <p>When I’m not knee-deep in terminal windows or project charts, I’m probably brushing up on legal theory, exploring how technology impacts the law, or just enjoying a bit of downtime and side-project chaos.</p>
              <p>For business enquiries, feel free to reach out at <a href="mailto:me@daniels-lab.com" class="text-info underline underline-offset-4 transition-all hover:text-blue-400">me@daniels-lab.com</a>.</p>
              <div class="flex flex-col gap-3 mt-4">
                <Button href="mailto:me@daniels-lab.com" variant="default">Business enquiries</Button>
                <Button href="https://www.youtube.com/watch?v=dQw4w9WgXcQ" target="_blank" variant="default">
                  Open my CV
                </Button>
              </div>
            </div>
          </Tabs.Content>

          <Tabs.Content value="skills">
            <div class="space-y-4">
              <Card.Title class="text-center">Skills and Certifications</Card.Title>
              <Accordion.Root type="single">
                <Accordion.Item value="general-skills" class="border-b-2 border-indigo-400">
                  <Accordion.Trigger class="font-semibold">General and IT Skills</Accordion.Trigger>
                  <Accordion.Content>
                    <ul class="list-disc pl-6 text-sm">
                      <li>Calm problem-solving skills</li>
                      <li>Home-Self-Hosting - Intermediate</li>
                      <li>Windows and Linux Administration - Intermediate</li>
                      <li>Basic networking knowledge and acquaintance with the UniFi ecosystem</li>
                    </ul>
                  </Accordion.Content>
                </Accordion.Item>
                <Accordion.Item value="languages" class="border-b-2 border-indigo-400">
                  <Accordion.Trigger class="font-semibold">Languages</Accordion.Trigger>
                  <Accordion.Content>
                    <ul class="list-disc pl-6 text-sm">
                      <li>Welsh - Native</li>
                      <li>English - Secondary</li>
                      <li>Wenglish? - Intermediate</li>
                    </ul>
                  </Accordion.Content>
                </Accordion.Item>
                <Accordion.Item value="certificates" class="border-b-2 border-indigo-400">
                  <Accordion.Trigger class="font-semibold">Certifications</Accordion.Trigger>
                  <Accordion.Content>
                    <ul class="list-disc pl-6 text-sm">
                      <li>
                        BSc (Hons) Professional Policing
                        <a
                          href="https://www.joiningthepolice.co.uk/application-process/ways-in-to-policing/professional-policing-degree-holder"
                          target="_blank"
                          class="text-blue-400 hover:underline"
                        >[show]
                        </a>
                      </li>
                      <li>Level 3 & 2 BTEC Pearsons Public Services</li>
                      <li>Level 2 BTEC Pearsons Information Creative Technology</li>
                      <li>Level 2 BTEC Pearsons Media & Photography</li>
                    </ul>
                  </Accordion.Content>
                </Accordion.Item>
              </Accordion.Root>
            </div>
          </Tabs.Content>

          <Tabs.Content value="setup">
            <div class="space-y-4">
              <Card.Title class="text-center">My Tinkering Setup</Card.Title>
              <div class="my-6 w-full overflow-y-auto">
                {#if loading}
                  <div class="text-center text-neutral-400">Loading...</div>
                {:else if error}
                  <div class="text-center text-red-400">{error}</div>
                {:else}
                  <table class="w-full border border-white/20 rounded-lg overflow-hidden">
                    <thead>
                      <tr class="bg-white/5">
                        <th class="border border-white/20 px-4 py-2 text-left font-bold">Type</th>
                        <th class="border border-white/20 px-4 py-2 text-left font-bold">Device</th>
                        <th class="border border-white/20 px-4 py-2 text-left font-bold">Specs / Details</th>
                      </tr>
                    </thead>
                    <tbody>
                      {#each setupData as item, i}
                        <tr class="{i % 2 === 1 ? 'bg-white/5' : ''}">
                          <td class="border border-white/20 px-4 py-2 text-left">{item.type}</td>
                          <td class="border border-white/20 px-4 py-2 text-left">{item.device}</td>
                          <td class="border border-white/20 px-4 py-2 text-left">{item.specs}</td>
                        </tr>
                      {/each}
                    </tbody>
                  </table>
                {/if}
              </div>
            </div>
          </Tabs.Content>
        </Tabs.Root>
      </Card.Content>
    </Card.Root>
  </div>
</div>
