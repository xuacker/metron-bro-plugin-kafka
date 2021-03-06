#
#  Licensed to the Apache Software Foundation (ASF) under one or more
#  contributor license agreements.  See the NOTICE file distributed with
#  this work for additional information regarding copyright ownership.
#  The ASF licenses this file to You under the Apache License, Version 2.0
#  (the "License"); you may not use this file except in compliance with
#  the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#

cmake_minimum_required(VERSION 2.8)
project(Plugin)
include(BroPlugin)
find_package(LibRDKafka)
find_package(OpenSSL)

if (LIBRDKAFKA_FOUND AND OPENSSL_FOUND)
  include_directories(BEFORE ${LibRDKafka_INCLUDE_DIR} ${OpenSSL_INCLUDE_DIR})
  bro_plugin_begin(APACHE KAFKA)
  bro_plugin_cc(src/KafkaWriter.cc)
  bro_plugin_cc(src/Plugin.cc)
  bro_plugin_cc(src/TaggedJSON.cc)
  bro_plugin_bif(src/kafka.bif)
  bro_plugin_dist_files(README CHANGES COPYING VERSION)
  bro_plugin_link_library(${LibRDKafka_LIBRARIES})
  bro_plugin_link_library(${LibRDKafka_C_LIBRARIES})
  bro_plugin_link_library(${OpenSSL_LIBRARIES})
  bro_plugin_end()

elseif (NOT LIBRDKAFKA_FOUND)
  message(FATAL_ERROR "LibRDKafka not found.")

elseif (NOT OPENSSL_FOUND)
  message(FATAL_ERROR "OpenSSL not found.")

endif ()
