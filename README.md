# Inside-my-Head-Sonic-Pi-Project
An exploration into the potential of Sonic Pi and Data and Code within the world of music.


# Inside My Head
notes = (ring :F4, :E4, :G4, :C5, :C5, :C4)
durations = [1, 0.5, 1, 1, 0.5, 0.5]
live_loop :fast_melody do
  tick
  play notes.look
  sleep durations.look
end
use_synth:pretty_bell
with_fx :reverb, room: 0.8 do
  synth :prophet, note: :E3, release: 4, cutoff: 70, amp: 0.6
end
sample:bass_thick_c
with_fx :echo, mix: 0.5, phase: 0.5 do
  play :C3, sustain: 5, release: 2, amp: 0.5
end
use_synth:hollow
with_fx :reverb, room: 0.8 do
  synth :piano, note: :C4, release: 4
  live_loop :fade_inout_synth do
    vol = (line 0, 1, inclusive: true, steps: 125).mirror
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    s = synth :dtri, cutoff: 80, note: :e3, sustain: 8, release: 0, amp: vol.tick(:v)
    puts "---------- Vol 1: #{vol.look(:v)} ----------"
    32.times do
      control s, note: notes.tick, note_slide: 0.005, amp_slide: 0.125, amp: vol.tick(:v)
      puts "---------- Vol 1: #{vol.look(:v)} ----------"
      sleep 0.25
    end
  end
end
sleep 8
live_loop :high_hat do
  sample :drum_cymbal_closed
  sleep 0.5
end
define :drum_pattern do
  8.times do
    sample :drum_tom_lo_soft, rate: 1.5 if one_in(2)
    sample :drum_snare_soft  if one_in(3)
    sleep 0.5
  end
end
live_loop :drums do
  drum_pattern
end
sample :tabla_ghe4
sleep 1
sample :bass_trance_c
sleep 1
sample :bass_trance_c
sleep 5
define :rnb_drums do
  sample :drum_bass_soft
  sleep 1.0
  sample :drum_snare_soft
  sleep 1.0
  sample :drum_bass_soft
  sleep 1.0
  sample :drum_bass_soft
  sleep 1.5
  sample :drum_snare_soft
  sleep 1.5
  sample :sn_dolf
  sleep 1
  sample :sn_dub
  sleep 1
end
sample :bass_trance_c
sleep 1
live_loop :rnb_drums_loop do
  rnb_drums
  sample :bd_ada
  sleep 2
  use_synth:pretty_bell
  with_fx :reverb, room: 0.8 do
    synth :prophet, note: :E3, release: 4, cutoff: 70, amp: 0.6
  end
end
live_loop :soft_hats do
  use_synth :dpulse
  nlen = [0.0625].choose
  with_fx :ring_mod, cutoff: 20 do
    2.times do
      play 2, release: rrand(0.03, 0.04)
      sleep nlen
    end
  end
end
define :fast_hats do
  14.times do
    sample :drum_bass_soft, amp: 0.5, rate: 1.2
    sleep 0.125
  end
end
live_loop :hi_hats do
  fast_hats
end
live_loop :mids,sync: :clock do
  tick
  mids ring(:c3,:d3,:e3,:e3).look,16
  sleep 16
end
define :leads do |notes, sustain|
  midi notes, port: :iac_driver_bus_4, sustain: sustain
end
live_loop :fade_inout_amen do
   vol = (line 0, 1, inclusive: true, steps: 24).mirror
   s = sample :loop_amen, beat_stretch: 4, amp: vol.tick
  puts "---------- Vol 1: #{vol.look} ----------"
  control s, amp_slide: 4, amp: vol.tick
  puts "---------- Vol 2: #{vol.look} ----------"
  sleep 4
end
live_loop :pad do
  sample :ambi_drone, rpitch: 5, amp: 0.5
  sleep 0.25
  sample :ambi_drone, rpitch: 2, amp: 0.5
  sleep 0.25
end
live_loop :sample do
  with_fx :slicer do
    with_fx:echo do
      with_fx :reverb do
        sample :ambi_choir, rpitch: 6, cutoff: 130, rate: 1
        sleep 5
        sample :ambi_choir, rpitch: 8, cutoff: 130, rate: -1
        sleep 5
        sample :ambi_choir, rpitch: 4, cutoff: 124, rate: -1
        sleep 2
        sample :ambi_choir, rpitch: 6, cutoff: 124, rate: -1
        sleep 5
      end
    end
  end
end


Log:

             "drum_snare_soft.flac"
 
{run: 371, time: 96.6612, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0207, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.6612, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 74.0, release: 0.6667}
 
{run: 371, time: 96.6612, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 96.6612, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 96.6612, thread: :live_loop_sample}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_choir.flac", {rpitch: 4, rate: -1.2599, lpf: 124}
 
{run: 371, time: 96.7029, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0219, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.7446, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 96.7446, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0265, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.7862, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0211, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.8279, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 96.8279, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0221, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.8279, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 96.8279, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 57.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8065}
 └─ "---------- Vol 1: 0.8064516129032258 ----------"
 
{run: 371, time: 96.8696, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0224, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.9112, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 96.9112, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0234, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.9529, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0247, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.9946, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 96.9946, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0227, dpulse_width_slide: 0.0}
 
{run: 371, time: 96.9946, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 59.0, release: 0.6667}
 
{run: 371, time: 96.9946, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 96.9946, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 96.9946, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 67.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8145}
 └─ "---------- Vol 1: 0.814516129032258 ----------"
 
{run: 371, time: 96.9946, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 97.0362, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0247, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.0779, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.0779, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0239, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.1196, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0229, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.1612, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 97.1612, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.1617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0206, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.1617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 64.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8226}
 └─ "---------- Vol 1: 0.8225806451612903 ----------"
 
{run: 371, time: 97.2034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.245, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.245, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0257, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.2867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0258, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.3284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 71.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8306}
 └─ "---------- Vol 1: 0.8306451612903225 ----------"
 
{run: 371, time: 97.3284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0261, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.3284, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 97.3284, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 97.3284, thread: :live_loop_rnb_drums_loop}
 ├─ synth :prophet, {note: 52.0, release: 2.6667, cutoff: 70, amp: 0.6}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac"
 
{run: 371, time: 97.3284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.3284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 97.37, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0243, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.4117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.4117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0218, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.4534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0203, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.495, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 76.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8387}
 └─ "---------- Vol 1: 0.8387096774193548 ----------"
 
{run: 371, time: 97.495, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0216, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.495, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.495, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 97.5367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0257, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.5784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0219, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.5784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.62, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.025, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.6617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0212, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.6617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 97.6617, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 97.6617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.6617, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 55.0, release: 0.6667}
 
{run: 371, time: 97.6617, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 97.6617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 69.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8468}
 └─ "---------- Vol 1: 0.8467741935483871 ----------"
 
{run: 371, time: 97.7034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0263, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.745, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0234, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.745, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.7867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0255, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.8284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 97.8284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.8284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.021, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.8284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 62.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8548}
 └─ "---------- Vol 1: 0.8548387096774194 ----------"
 
{run: 371, time: 97.87, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.025, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.9117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.9117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0233, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.9534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0239, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.995, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 52.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8629}
 └─ "---------- Vol 1: 0.8629032258064516 ----------"
 
{run: 371, time: 97.995, thread: :live_loop_sample}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_choir.flac", {rpitch: 6, rate: -1.4142, lpf: 124}
 
{run: 371, time: 97.995, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 97.995, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 97.995, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0244, dpulse_width_slide: 0.0}
 
{run: 371, time: 97.995, thread: :live_loop_rnb_drums_loop}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 97.995, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 98.0367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0213, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.0784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0229, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.0784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.12, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0224, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.1617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 98.1617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0235, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.1617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.1617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 74.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.871}
 └─ "---------- Vol 1: 0.8709677419354839 ----------"
 
{run: 371, time: 98.2034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0211, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.245, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.245, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.024, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.2867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0203, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.3284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 98.3284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.3284, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 98.3284, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 57.0, release: 0.6667}
 
{run: 371, time: 98.3284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 59.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.879}
 └─ "---------- Vol 1: 0.8790322580645161 ----------"
 
{run: 371, time: 98.3284, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 98.3284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0208, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.37, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0254, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.4117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0243, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.4117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.4534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0255, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.495, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 98.495, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.495, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 55.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8871}
 └─ "---------- Vol 1: 0.8870967741935484 ----------"
 
{run: 371, time: 98.495, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0222, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.5367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0248, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.5784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0232, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.5784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.62, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0204, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.6617, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 67.0, release: 0.6667}
 
{run: 371, time: 98.6617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 57.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.8952}
 └─ "---------- Vol 1: 0.8951612903225806 ----------"
 
{run: 371, time: 98.6617, thread: :live_loop_drums}
 ├─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
 │           "drum_tom_lo_soft.flac", {rate: 1.5}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 98.6617, thread: :live_loop_fade_inout_amen}
 ├─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
 │           "loop_amen.flac", {beat_stretch: 4, amp: 0.7826, rate: 0.6575}
 ├─ "---------- Vol 1: 0.7826086956521738 ----------"
 ├─ control node 118393, {amp_slide: 2.6667, amp: 0.8261}
 └─ "---------- Vol 2: 0.8260869565217391 ----------"
 
{run: 371, time: 98.6617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.6617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 98.6617, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 98.6617, thread: :live_loop_rnb_drums_loop}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac"
 
{run: 371, time: 98.6617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0256, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.7034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0224, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.745, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0202, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.745, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.7867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0222, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.8284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 98.8284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 67.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9032}
 └─ "---------- Vol 1: 0.9032258064516129 ----------"
 
{run: 371, time: 98.8284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.8284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0222, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.87, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0242, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.9117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.9117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.9534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0242, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.995, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0242, dpulse_width_slide: 0.0}
 
{run: 371, time: 98.995, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 64.0, release: 0.6667}
 
{run: 371, time: 98.995, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 64.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9113}
 └─ "---------- Vol 1: 0.9112903225806451 ----------"
 
{run: 371, time: 98.995, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 98.995, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 98.995, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 98.995, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 99.0367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0228, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.0784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.0784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0238, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.12, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.1617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.1617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 71.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9194}
 └─ "---------- Vol 1: 0.9193548387096774 ----------"
 
{run: 371, time: 99.1617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0228, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.1617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 99.2034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.025, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.245, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0232, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.245, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.2867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0216, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.3284, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 99.3284, thread: :live_loop_rnb_drums_loop}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac"
 
{run: 371, time: 99.3284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.3284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 99.3284, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 99.3284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 76.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9274}
 └─ "---------- Vol 1: 0.9274193548387096 ----------"
 
{run: 371, time: 99.3284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0208, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.37, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0203, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.4117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.4117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0211, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.4534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0237, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.495, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.495, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 69.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9355}
 └─ "---------- Vol 1: 0.9354838709677419 ----------"
 
{run: 371, time: 99.495, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.021, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.495, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 99.5367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0207, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.5784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.5784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.62, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0249, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.6617, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 99.6617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 62.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9435}
 └─ "---------- Vol 1: 0.9435483870967741 ----------"
 
{run: 371, time: 99.6617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0223, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.6617, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 99.6617, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 71.0, release: 0.6667}
 
{run: 371, time: 99.6617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 99.6617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.7034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0214, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.745, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0231, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.745, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.7867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0237, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.8284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0264, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.8284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.8284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 52.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9516}
 └─ "---------- Vol 1: 0.9516129032258064 ----------"
 
{run: 371, time: 99.8284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 99.87, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0245, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.9117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0223, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.9117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.9534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.995, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 74.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9597}
 └─ "---------- Vol 1: 0.9596774193548386 ----------"
 
{run: 371, time: 99.995, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 99.995, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 99.995, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0214, dpulse_width_slide: 0.0}
 
{run: 371, time: 99.995, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 99.995, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 76.0, release: 0.6667}
 
{run: 371, time: 100.0367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0205, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.0784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0201, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.0784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.12, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0238, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.1617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0226, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.1617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 59.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9677}
 └─ "---------- Vol 1: 0.967741935483871 ----------"
 
{run: 371, time: 100.1617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.1617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 100.2034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.024, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.245, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0236, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.245, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.2867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0211, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.3284, thread: :live_loop_rnb_drums_loop}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 100.3284, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 100.3284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0235, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.3284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 100.3284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 55.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9758}
 └─ "---------- Vol 1: 0.9758064516129032 ----------"
 
{run: 371, time: 100.3284, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 100.3284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.37, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0249, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.4117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.4117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.021, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.4534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0257, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.495, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 57.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9839}
 └─ "---------- Vol 1: 0.9838709677419355 ----------"
 
{run: 371, time: 100.495, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.495, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 100.495, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0206, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.5367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0261, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.5784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0245, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.5784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.62, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0253, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.6617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0208, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.6617, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 100.6617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.6617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 67.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9919}
 └─ "---------- Vol 1: 0.9919354838709677 ----------"
 
{run: 371, time: 100.6617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 100.6617, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 69.0, release: 0.6667}
 
{run: 371, time: 100.7034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0252, dpulse_width_slide: 0.0}
 
=> Stopping all runs...

=> Stopping run 371

{run: 371, time: 100.745, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.745, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0224, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.7867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0221, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.8284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 100.8284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.025, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.8284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.8284, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 64.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 1.0}
 └─ "---------- Vol 1: 1.0 ----------"
 
{run: 371, time: 100.87, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0203, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.9117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0264, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.9117, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.9534, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0265, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.995, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 100.995, thread: :live_loop_drums}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_tom_lo_soft.flac", {rate: 1.5}
 
{run: 371, time: 100.995, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 100.995, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 100.995, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0213, dpulse_width_slide: 0.0}
 
{run: 371, time: 100.995, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 71.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 1.0}
 └─ "---------- Vol 1: 1.0 ----------"
 
{run: 371, time: 101.0367, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0256, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.0784, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 101.0784, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0232, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.12, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0243, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.1617, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0222, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.1617, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 101.1617, thread: :live_loop_fade_inout_synth}
 ├─ control node 118154, {note: 76.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9919}
 └─ "---------- Vol 1: 0.9919354838709677 ----------"
 
{run: 371, time: 101.1617, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
{run: 371, time: 101.2034, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0261, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.245, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 101.245, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0241, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.2867, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0216, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.3284, thread: :live_loop_fast_melody}
 └─ synth :beep, {note: 62.0, release: 0.6667}
 
{run: 371, time: 101.3284, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0226, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.3284, thread: :live_loop_hi_hats}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_bass_soft.flac", {amp: 0.5, rate: 1.2}
 
{run: 371, time: 101.3284, thread: :live_loop_drums}
 ├─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
 │           "drum_tom_lo_soft.flac", {rate: 1.5}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_snare_soft.flac"
 
{run: 371, time: 101.3284, thread: :live_loop_fade_inout_amen}
 ├─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
 │           "loop_amen.flac", {beat_stretch: 4, amp: 0.8696, rate: 0.6575}
 ├─ "---------- Vol 1: 0.8695652173913043 ----------"
 ├─ control node 118630, {amp_slide: 2.6667, amp: 0.913}
 └─ "---------- Vol 2: 0.9130434782608695 ----------"
 
{run: 371, time: 101.3284, thread: :live_loop_high_hat}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "drum_cymbal_closed.flac"
 
{run: 371, time: 101.3284, thread: :live_loop_rnb_drums_loop}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "sn_dolf.flac"
 
{run: 371, time: 101.3284, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 5, amp: 0.5, rate: 1.3348}
 
{run: 371, time: 101.3284, thread: :live_loop_fade_inout_synth}
 ├─ synth :dtri, {cutoff: 80, note: 52.0, sustain: 5.3333, release: 0.0, amp: 0.9839}
 ├─ "---------- Vol 1: 0.9838709677419355 ----------"
 ├─ control node 118627, {note: 71.0, note_slide: 0.0033, amp_slide: 0.0833, amp: 0.9758}
 └─ "---------- Vol 1: 0.9758064516129032 ----------"
 
{run: 371, time: 101.37, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0215, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.4117, thread: :live_loop_soft_hats}
 └─ synth :dpulse, {note: 2.0, release: 0.0262, dpulse_width_slide: 0.0}
 
{run: 371, time: 101.495, thread: :live_loop_pad}
 └─ sample "/Applications/Sonic Pi.app/Contents/Resources/etc/samples",
             "ambi_drone.flac", {rpitch: 2, amp: 0.5, rate: 1.1225}
 
=> Completed run 371

=> All runs completed

=> Pausing SuperCollider Audio Server
