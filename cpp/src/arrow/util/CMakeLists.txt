# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

#
# arrow_util
#

# Headers: top level
arrow_install_all_headers("arrow/util")

#
# arrow_test_main
#

if(WIN32)
  # This manifest enables long file paths on Windows 10+
  # See https://docs.microsoft.com/en-us/windows/win32/fileio/naming-a-file#enable-long-paths-in-windows-10-version-1607-and-later
  if(MSVC)
    set(IO_UTIL_TEST_SOURCES io_util_test.cc io_util_test.manifest)
  else()
    set(IO_UTIL_TEST_SOURCES io_util_test.cc io_util_test.rc)
  endif()
else()
  set(IO_UTIL_TEST_SOURCES io_util_test.cc)
endif()

set(ARROW_UTILITY_TEST_LINK_LIBS Boost::headers)
if(ARROW_USE_XSIMD)
  list(APPEND ARROW_UTILITY_TEST_LINK_LIBS ${ARROW_XSIMD})
endif()
if(ARROW_WITH_OPENTELEMETRY)
  list(APPEND ARROW_UTILITY_TEST_LINK_LIBS ${ARROW_OPENTELEMETRY_LIBS})
endif()
add_arrow_test(utility-test
               SOURCES
               align_util_test.cc
               atfork_test.cc
               byte_size_test.cc
               byte_stream_split_test.cc
               cache_test.cc
               checked_cast_test.cc
               compression_test.cc
               decimal_test.cc
               float16_test.cc
               fixed_width_test.cc
               formatting_util_test.cc
               key_value_metadata_test.cc
               hashing_test.cc
               int_util_test.cc
               ${IO_UTIL_TEST_SOURCES}
               iterator_test.cc
               list_util_test.cc
               logger_test.cc
               logging_test.cc
               math_test.cc
               queue_test.cc
               range_test.cc
               ree_util_test.cc
               reflection_test.cc
               rows_to_batches_test.cc
               secure_string_test.cc
               small_vector_test.cc
               span_test.cc
               stl_util_test.cc
               string_test.cc
               tdigest_test.cc
               test_common.cc
               time_test.cc
               tracing_test.cc
               trie_test.cc
               uri_test.cc
               utf8_util_test.cc
               value_parsing_test.cc
               EXTRA_LINK_LIBS
               ${ARROW_UTILITY_TEST_LINK_LIBS})

add_arrow_test(async-utility-test
               SOURCES
               async_generator_test.cc
               async_util_test.cc
               test_common.cc)

add_arrow_test(bit-utility-test
               SOURCES
               bit_block_counter_test.cc
               bit_util_test.cc
               rle_encoding_test.cc)

add_arrow_test(threading-utility-test
               SOURCES
               cancel_test.cc
               counting_semaphore_test.cc
               future_test.cc
               task_group_test.cc
               test_common.cc
               thread_pool_test.cc)

add_arrow_test(crc32-test
               SOURCES
               crc32_test.cc
               EXTRA_LINK_LIBS
               Boost::headers)

add_arrow_benchmark(bit_block_counter_benchmark)
add_arrow_benchmark(bit_util_benchmark)
add_arrow_benchmark(bitmap_reader_benchmark)
add_arrow_benchmark(cache_benchmark)
add_arrow_benchmark(compression_benchmark)
add_arrow_benchmark(decimal_benchmark)
add_arrow_benchmark(hashing_benchmark)
add_arrow_benchmark(int_util_benchmark)
add_arrow_benchmark(machine_benchmark)
add_arrow_benchmark(queue_benchmark)
add_arrow_benchmark(range_benchmark)
add_arrow_benchmark(small_vector_benchmark)
add_arrow_benchmark(tdigest_benchmark)
add_arrow_benchmark(thread_pool_benchmark)
add_arrow_benchmark(trie_benchmark)
set(ARROW_BENCHMARK_UTF8_UTIL_LINK_LIBS)
if(ARROW_USE_XSIMD)
  list(APPEND ARROW_BENCHMARK_UTF8_UTIL_LINK_LIBS ${ARROW_XSIMD})
endif()
add_arrow_benchmark(utf8_util_benchmark EXTRA_LINK_LIBS
                    ${ARROW_BENCHMARK_UTF8_UTIL_LINK_LIBS})
add_arrow_benchmark(value_parsing_benchmark)
