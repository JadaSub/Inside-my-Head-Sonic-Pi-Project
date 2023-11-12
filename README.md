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
